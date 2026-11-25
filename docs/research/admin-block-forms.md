# Admin forms for Block types

Research date: 2026-11-25. Terms follow the CMS glossary: Site, Client, Page, Block.

## Question

How does the admin show the correct form for each Block type?

Options:

- (a) A JSON Schema from the API, rendered by a Svelte 5 form renderer.
- (b) Hand-written forms, one per Block type.
- (c) A small field-type registry. C# attributes produce field descriptors. One Svelte component exists per field type.

## Short answer

Option (c) fits the task best. The Client edits a fixed set of field types. A general form library adds a dependency that does not solve rich text or image fields. Option (a) works for simple types, but polymorphic Block types are not verified. Option (b) is fast for five or six Block types and grows with each new type. Revisit option (a) only if the field model grows to conditional or nested forms.

## Findings

### 1. .NET schema generation

- `System.Text.Json.Schema.JsonSchemaExporter` is a static class. It has `GetJsonSchemaAsNode(JsonSerializerOptions, Type, JsonSchemaExporterOptions)` and `GetJsonSchemaAsNode(JsonTypeInfo, JsonSchemaExporterOptions)`. The reference page lists .NET 9 among its monikers. [1]
- The exporter returns a `JsonNode`. `JsonSchemaExporterOptions.TransformSchemaNode` runs for each generated schema node. Confirmed only from a search snippet, not from the page body. [2][3]
- The extraction guide shows `[Description]` text copied into the schema. Confirmed from a search snippet only. [3]
- The exporter treats nullable and non-nullable properties differently. Required fields depend on constructor parameter optionality. Confirmed from a search snippet only. [3]
- `[JsonDerivedType]` and `[JsonPolymorphic]` exist since .NET 7. [4]
- Whether the exporter emits `oneOf` or a discriminator for these types: UNVERIFIED.
- Rich text, image and link fields have no built-in JSON Schema type. A `TransformSchemaNode` callback or a custom extension key is needed. This is a design inference, not a documented pattern.
- .NET 11 adds closed-hierarchy inference for polymorphism. [4] Relevant only if the project targets .NET 11.

### 1b. Microsoft.AspNetCore.OpenApi

- The package provides OpenAPI document generation. It is used with `AddOpenApi` and `MapOpenApi`. The overview lists it for .NET 9 and later. [5]
- Schema transformers receive `context.JsonTypeInfo` and can change any schema. [6]
- .NET 10 adds `GetOrCreateSchemaAsync` for transformers. [6]
- Polymorphism: a .NET 9 RC1 report says the generated schema used `anyOf` where `oneOf` was expected. Whether this is fixed in the final release is UNVERIFIED. [7]

### 2. Svelte 5 JSON Schema form renderers

Verified from the npm registry (`npm view`) and the GitHub API, 2026-11-25.

| Package | Latest | Published | License | Last push | Svelte peer |
|---|---|---|---|---|---|
| `@sjsf/form` (repo `x0k/svelte-jsonschema-form`) | 3.10.0 | 2026-11-14 | npm: MIT AND Apache-2.0. GitHub: MIT | 2026-11-24, not archived, 4 open issues | `^5.34.8` |
| `sveltekit-superforms` | 3.1.0 | 2026-10-29 | MIT | 2026-10-29, 90 open issues | `^5.56.4` |
| `svelte-jsonschema-form` (old name) | npm 404 | n/a | n/a | n/a | n/a |

Sources: [8][9][10]

Notes:

- A search result gave 3.7.0 as the newest `@sjsf/form` version. The npm registry gives 3.10.0. The registry result is used here.
- The `@sjsf/form` repo describes itself as a Svelte 5 library for JSON Schema forms. [10]
- A search snippet calls it an unofficial port of react-jsonschema-form. Not verified in the README. UNVERIFIED.
- The peer range confirms Svelte 5. Whether the internals use runes is UNVERIFIED.
- `sveltekit-superforms` is a form and validation library. Its peer list is validator adapters (Zod, Valibot and others). It has a `jsonSchema` option that overrides the adapter schema. [11][12] It does not render forms from a schema. Not a renderer candidate.

### 3. Other options

- Option (c), the registry: one descriptor type per field kind, one Svelte component per kind, and one reflection pass over the attributes. Estimate: 8 to 12 field kinds. No external dependency. Estimate, not sourced.
- Option (b), hand-written: one Svelte form per Block type. Each new Block type needs admin code. Estimate, not sourced.

## Comparison

| Criterion | (a) JSON Schema + renderer | (b) Hand-written per Block | (c) Field registry |
|---|---|---|---|
| Fit with C# attributes | Good for simple types. Polymorphism UNVERIFIED | Manual mapping | Direct: attributes become descriptors |
| Svelte 5 support | Yes for `@sjsf/form` (peer range) | Native | Native |
| Maintenance | Depends on a third-party package | Team code | Team code, small surface |
| Custom code | Renderer overrides for rich text and image | One form per Block type | One component per field kind |
| Client UX | Generic, may be inconsistent | Best per Block, slow to add | Consistent, editor-friendly |

## Recommendation

Choose option (c).

Reasons:

- The field set is small and fixed. The brief lists text, rich text, image, link and lists.
- Rich text and image need custom components in every option.
- A library adds a dependency whose polymorphism and discriminator output is not verified.
- Attributes stay the single source of truth.

Suggested shape: the API exposes a field descriptor list per Block type. The admin maps each descriptor to one component. Keep JSON Schema export as a later option.

Open risks:

- The exporter output for `[JsonDerivedType]` is UNVERIFIED. A short spike should test it before any decision on option (a).
- The OpenAPI `anyOf` or `oneOf` issue is UNVERIFIED as fixed.
- The rich text and image contract is not yet defined.
- `@sjsf/form` has a license mismatch between npm and GitHub. Check before any adoption.
- The project's .NET target version is not known. Version-specific claims are marked.

## Sources

1. https://learn.microsoft.com/en-us/dotnet/api/system.text.json.schema.jsonschemaexporter
2. https://learn.microsoft.com/pl-pl/dotnet/api/system.text.json.schema.jsonschemaexporteroptions.transformschemanode?view=net-8.0 (search snippet only)
3. https://learn.microsoft.com/lb-lu/DOTNET/standard/serialization/system-text-json/extract-schema (search snippet only)
4. https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/polymorphism
5. https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/overview?view=aspnetcore-11.0
6. https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/customize-openapi?view=aspnetcore-11.0
7. https://github.com/dotnet/aspnetcore/issues/57982 (search snippet only)
8. https://registry.npmjs.org/@sjsf/form (npm view, dist-tags and time)
9. https://registry.npmjs.org/sveltekit-superforms (npm view, dist-tags and time)
10. https://api.github.com/repos/x0k/svelte-jsonschema-form
11. https://api.github.com/repos/ciscoheat/sveltekit-superforms
12. https://superforms.rocks/api (search snippet only)
