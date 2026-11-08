# Qdrant Cloud MCP server

List collections, query a supplied vector, and only after reviewing point IDs approve an upsert into the same collection. Re-query to inspect provider results; dimensions and collection access remain provider-enforced.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `QDRANT_API_KEY`
- `QDRANT_HOST`



Credential help: https://raw.githubusercontent.com/qdrant/qdrant/master/docs/redoc/master/openapi.json

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call list_collections '{}'
```

## Use with an MCP client

Launch `node /absolute/path/qdrant-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| list_collections | Read | List collections on the configured Qdrant Cloud cluster. |
| query_points | Read | Read nearest points for a numeric query vector. Vector dimension must match the selected collection. |
| upsert_points | Explicit approval required | Explicitly approved point insertion/update. Existing IDs are overwritten; acknowledged is not proof an asynchronous operation completed. |

## Concrete workflow

List collections, query a supplied vector, and only after reviewing point IDs approve an upsert into the same collection. Re-query to inspect provider results; dimensions and collection access remain provider-enforced.

1. list_collections
2. query_points
3. upsert_points

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-qdrant-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/qdrant/).

## Provenance and maintenance

- https://raw.githubusercontent.com/qdrant/qdrant/master/docs/redoc/master/openapi.json

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
