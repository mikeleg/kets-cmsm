# Translation per Field, one Page structure for all Languages

The MVP has only Italian, but the data is ready for more **Languages**. A **Page** has one list of **Blocks** for all Languages. Each text value stores one value for each Language, for example `{"it": "Chi siamo"}`. We chose this so a new Language needs no data migration and the Client only translates Fields. The alternative was one list of Blocks for each Language. We did not choose it, because the structures drift apart and someone must keep them aligned by hand.

## Consequences

- The kind of **Field** decides if it is translatable. Text, long text, formatted text, link label and image alt text are translatable. The image file, the link target and the choice are the same for all Languages.
- Slugs, SEO fields, Menu labels and Site settings texts use the same value for each Language.
- The default Language has no URL prefix. The other Languages will have one, for example `/en/`.
- The public API takes a `lang` parameter from the MVP on.
- Draft and Published are one state for each Page, for all Languages.
