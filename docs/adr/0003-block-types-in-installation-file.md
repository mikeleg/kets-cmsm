# Site-specific Block types in a file of the installation

The base set of **Block** types is in the CMS. A Block type for one **Site** is in a file of its installation (JSON or YAML with the fields), not in C# code. We chose this because each installation runs the same CMS image (ADR 0001). C# code for one Site would make that installation a fork.

## Consequences

- The admin builds each form from field descriptors. The descriptors come from the CMS for the base set and from the file for the other Block types.
- The content of a Block is in a JSON column. One table for each Block type does not work, because each installation would need its own migrations.
