# A change to a Block type never deletes content

A developer changes the Block types of a **Site** in the file of its **Installation** (ADR 0003). A change to that file never deletes saved content and never takes a **Page** off the Site. We chose this so a mistake in the file costs no data and a deploy never breaks a live Site.

## Consequences

- A removed **Field** keeps its value in the saved JSON. The API and the admin ignore it. The value comes back if the Field comes back with the same key.
- A new Field is empty in saved Blocks. If it is required, the admin marks those Blocks "Da completare", and the Page stays online.
- A removed Block type keeps its Blocks. The public API does not return them. The admin shows them as not editable, and the Client can delete them.
- The key of a Field cannot change in the MVP. A new key is a new Field.
- A change of the kind of a Field with the same key is an error, and the CMS does not start.
