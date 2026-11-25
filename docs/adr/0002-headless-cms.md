# Headless CMS with one frontend per Site

The CMS keeps the content and gives it through an HTTP API with an OpenAPI contract. It does not render the pages of a **Site**. Each Site has its own frontend, in any technology, that reads the API at each request (SSR) with a cache. We chose this so the design of each Site stays free, and so a Client preview needs no build pipeline. The CMS admin is a separate SvelteKit app on a .NET Minimal API.
