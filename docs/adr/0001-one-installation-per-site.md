# One installation per Site

Each **Site** has its own installation of the CMS: its own API, admin and SQLite database. We do not use one shared installation for all Sites. One installation per Site keeps each Client's data apart and lets each Site change at its own pace. SQLite fits because a Site has few writers and low traffic.

## Consequences

- An update of the CMS must reach every installation. Each installation updates on its own.
- There is no central view of all Sites.
