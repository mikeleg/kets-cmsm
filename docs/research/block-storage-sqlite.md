# Block Storage in SQLite from a .NET Minimal API

Research date: 2026-11-25. Source versions are listed in section 9.

## 1. Question

How should the CMS store the field values of Blocks of different Block types in SQLite? The options are EF Core JSON columns, one table per Block type, and hand-written SQL with Dapper. The comparison covers migrations, multilanguage, and reading the Blocks of a Page in order.

## 2. Short answer

Use one shared Block table for the list of Blocks on each Page. It holds PageId, Position, and the Block type name. Store the fields of each Block type in its own typed table, mapped with EF Core. Field changes then follow normal EF Core migrations, which SQLite supports for the common cases. Use a JSON payload only if Block types change very often and need no SQL filtering. Multilanguage is the open design point that can change this choice.

## 3. Options compared

### 3.1 Option 1: EF Core JSON columns

EF Core maps a .NET object to one JSON column. SQLite support arrived in EF Core 8 [1]. The EF Core 8 page says the JSON mapping "now also applies to SQLite" and covers queries, projections and updates [1].

Pros:
- EF Core maps a JSON document to a .NET type with no hand-written JSON code [1].
- Queries can filter and sort by elements of the document [1].
- The page shows that `SaveChanges` updates JSON documents in the normal way [2].

Cons:
- One JSON-mapped property maps one .NET type. Blocks of many Block types need one property per Block type, or a common base type. The owned type page states owned types "cannot have inheritance hierarchies" [3].
- The complex type page says "EF Core does not yet support inheritance for complex types" [4]. Complex types are the preferred JSON mapping in EF Core 10 and later [4][5].
- Owned JSON collections without an explicit key are obsolete in EF Core 11 and raise a warning [5].
- Complex collections on a relational provider must map to JSON with `ToJson` [4].
- The payload has no schema in SQLite. Stored JSON is not checked by the database. Inference: EF migrations compare the model with a snapshot and generate schema changes [6]. The migrations overview does not describe changes to stored JSON content [6].
- EF Core 8 states that the primitive collection convention maps `IEnumerable<T>` of primitive types to JSON [7]. The EF Core 8 examples use SQL Server. Whether the convention applies on SQLite is UNVERIFIED.

Variant 1b: keep the Block list in EF Core and store the payload as a JSON text column. Serialization then uses `System.Text.Json` in C#. EF Core does not map the payload. The team owns the shape and the version. The value converter route was not read. Its behavior on SQLite is UNVERIFIED.

Variant 1c: map each Block type to an entity type under TPH with a `ToJson` property. Whether `ToJson` works per subtype under TPH is UNVERIFIED.

### 3.2 Option 2: one table per Block type

EF Core maps a type hierarchy with table-per-type (TPT). Each type gets its own table. Derived tables store a foreign key to the base table [8]. For this CMS, the base table is the shared Block table, and each Block type is a derived table.

Pros:
- Each field has its own typed column. SQLite checks the column type.
- The base table holds the order of Blocks through PageId and Position. This needs no JSON functions [8].
- EF Core migrations handle add, rename and drop of columns [6][9].

Cons:
- The EF Core inheritance page says "In many cases, TPT shows inferior performance when compared to TPH" [8].
- The same page says "Use TPT only if constrained to do so by external factors" [8].
- Reading all Blocks of a Page needs a join to each Block type table. The read cost grows with the number of Block types.
- The table count grows with each new Block type.

### 3.3 Option 3: hand-written SQL with Dapper

Variant 3a: one shared table with a JSON text payload. Code reads the payload with Dapper and deserializes it in C#. The SQLite JSON functions are built in from version 3.38.0 [10].

Variant 3b: typed tables, one per Block type, written by hand.

Pros:
- Full control of SQL and of the mapping code.
- Dapper supports multi-mapping with `splitOn`. The README shows a join that maps one row to two objects [11].
- Dapper has no built-in JSON handling. The README does not mention JSON [11]. The team writes the deserialization code.

Cons:
- No migration tool. The team writes the DDL and the data migration scripts.
- SQLite restricts `ALTER TABLE`. Other changes need the 12-step table rebuild procedure by hand [12].
- Mapping code grows with each Block type.

## 4. Comparison table

| Row | Option 1: EF Core JSON | Option 2: table per Block type (TPT) | Option 3: Dapper |
| --- | --- | --- | --- |
| Query fit | Filters on JSON paths. Index needs an expression index [13]. | Joins per Block type. Ordering uses indexed Position. | Full SQL. JSON1 functions built in [10]. |
| Typing in C# | One .NET type per JSON property. Inheritance limited [3][4]. | Typed entity per Block type [8]. | Hand-written mapping and DTOs. |
| Schema per Block type | None. Shape lives in JSON. | One table per Block type. | One table per Block type, written by hand. |
| Migrations on field change | Schema does not change. Data changes are manual. | EF migrations. Drop rebuilds the table [6]. | Manual DDL and data scripts [12]. |
| Multilanguage fit | Per-language keys inside the JSON document. | Per-language rows in a translation table. | Either design, written by hand. |
| SQL/Dapper effort | Low for EF users. | Medium. EF maps the types. | High. Mapping and migrations by hand. |
| EF Core version dependence | Needs EF Core 8 or later for SQLite JSON [1]. | Works across EF Core versions. TPT is long documented [8]. | None. Dapper does not depend on EF Core. |

## 5. Migrations

### 5.1 Option 1: JSON columns

- Add a field: the database needs no change. Old documents lack the field. Code must handle a missing value. Inference from [6] and [2].
- Rename or remove a field: stored documents keep the old key until the team rewrites them. EF Core does not rewrite stored JSON. Inference from [6]. The team needs a data migration.
- Version the payload: store a schema version with each document. The read code upgrades old versions. This is a design choice. No source is cited.

### 5.2 Option 2: typed tables with EF Core migrations

- Add a field: EF Core `AddColumn` is supported on SQLite [6]. SQLite restricts the new column. It cannot be a primary key or unique. A NOT NULL column needs a default that is not NULL [12].
- Rename a field: EF Core `RenameColumn` is supported on SQLite [6]. SQLite supports `RENAME COLUMN` [12].
- Remove a field: EF Core `DropColumn` runs as a table rebuild on SQLite [6]. SQLite `DROP COLUMN` fails if the column is indexed, unique, a key, or used in a view or trigger [12]. SQLite `DROP COLUMN` "removes a column and purges its data" [12]. Test on a copy of the database before release.
- Change a field type: EF Core `AlterColumn` runs as a table rebuild [6].

### 5.3 Option 3: manual migrations

- Add a field: `ALTER TABLE ... ADD COLUMN` with the SQLite limits in [12].
- Other changes: use the 12-step procedure in [12]. It creates a new table, copies the rows, drops the old table, and renames the new one.
- SQLite 3.53.0 added `ALTER COLUMN ... SET NOT NULL` and `DROP NOT NULL` [12]. The team must confirm the bundled SQLite version before it relies on this.

## 6. Multilanguage

These are design notes. The sources do not cover multilanguage.

- Option 1, per-language JSON keys: a field stores one object with a key per language. Example shape: `{"title": {"it": "...", "en": "..."}}`. Adding a language needs no schema change. The payload shape changes, so the version field matters. Queries per language use `json_extract` [10].
- Option 2, per-language rows: a translation table per Block type, keyed by BlockId and language code. Foreign keys enforce the link to the Block [8]. Adding a language adds rows, not columns.
- Option 3: either shape, written by hand.

## 7. Recommendation

Choose Option 2: a shared Block table with typed tables per Block type, mapped with EF Core. Reasons:
- Block fields are fixed in C# code. Typed tables match that model.
- EF Core migrations handle add and rename on SQLite. Drops use table rebuilds [6].
- Reading a Page's Blocks in order needs only the indexed PageId and Position columns. It needs no JSON functions.
- Per-language rows keep translations typed and linked by foreign key.

Risks of this choice:
- The table count grows with each Block type.
- TPT reads cost more than TPH reads. The docs advise TPT only when forced [8]. The team must measure with real data volume.
- Drop-column migrations rebuild the table on SQLite [6]. A bad migration can lose data. Test on a copy before each release.
- EF Core versions change. Pin the EF Core version and read its breaking changes before upgrading [9].

## 8. Open questions

- How many Block types will the team define, and how often will their fields change?
- Will the CMS query Blocks across Block types, for example for search? This decides the TPT cost.
- Which languages are needed, and are translations per Block or per Page?
- Which EF Core version does production use? EF Core 10 is the stable release listed in [9]. The EF Core 11 breaking changes page is dated 2026-11-23 [5]. Its GA status on 2026-11-25 is UNVERIFIED.
- Does the team want the Block list in one table with a discriminator and TPH for the common fields? This was not tested.
- Does a per-subtype `ToJson` property work under TPH? UNVERIFIED.
- Does EF Core primitive collection mapping apply to SQLite? UNVERIFIED.
- Is `json_extract` deterministic, so it can appear in an index? The JSON1 page did not state this. UNVERIFIED.

## 9. Sources

Checked versions: EF Core 8, 10 and 11 pages as cited. SQLite 3.54.1 (current version on the download page, release date not shown). Dapper: README on the default branch. Read on 2026-11-25.

1. EF Core 8 what's new, JSON columns for SQLite. https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-8.0/whatsnew (ms.date not shown, read 2026-11-25)
2. EF Core 7 what's new, JSON columns (updates). https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-7.0/whatsnew (read 2026-11-25)
3. Owned entity types. https://learn.microsoft.com/en-us/ef/core/modeling/owned-entities (updated_at 2026-08-05, read 2026-11-25)
4. Complex types, EF Core 10 and 11 features. https://learn.microsoft.com/en-us/ef/core/modeling/complex-types (updated_at 2026-08-05, read 2026-11-25)
5. Breaking changes in EF Core 11. https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/breaking-changes (updated_at 2026-11-23, read 2026-11-25)
6. EF Core migrations overview and SQLite limitations. https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/ (updated_at 2026-08-11) and https://learn.microsoft.com/en-us/ef/core/providers/sqlite/limitations (updated_at 2026-10-28). Both read 2026-11-25.
7. EF Core 8 what's new, primitive collections. https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-8.0/whatsnew (read 2026-11-25)
8. EF Core inheritance (TPH, TPT, TPC). https://learn.microsoft.com/en-us/ef/core/modeling/inheritance (updated_at 2025-06-14, read 2026-11-25)
9. EF Core releases and planning. https://learn.microsoft.com/en-us/ef/core/what-is-new/ (updated_at 2025-11-20, read 2026-11-25)
10. SQLite JSON1 extension. https://www.sqlite.org/json1.html (read 2026-11-25)
11. Dapper README. https://github.com/DapperLib/Dapper (read 2026-11-25)
12. SQLite ALTER TABLE. https://www.sqlite.org/lang_altertable.html (read 2026-11-25)
13. SQLite generated columns and expression indexes. https://www.sqlite.org/gencol.html and https://www.sqlite.org/expridx.html (read 2026-11-25)

Not read: https://learn.microsoft.com/en-us/ef/core/providers/sqlite/json-columns and https://learn.microsoft.com/en-us/ef/core/modeling/primitive-collections returned HTTP 404 on 2026-11-25. The value converter page was not read.
