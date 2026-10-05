# MCP recovery and update

This is the single current MCP recovery/update guide.

## Non-negotiable invariants

1. Never modify Caddy during MCP recovery or update.
   Do not edit a Caddyfile, reload/restart/stop Caddy, change its task, change ingress, or move a route.
2. Never modify OAuth/auth state during MCP recovery or update.
   Do not copy, merge, recreate, rotate, delete, move, rename, or replace an OAuth store. Do not require re-login as part of recovery.
3. Keep every MCP binding on its existing backend port.
   Recovery/update is in-place with respect to binding identity and port. Do not allocate a temporary/new port and do not perform an ingress cutover.
4. Change only the MCP runtime that is actually being restored or updated.
   Preserve its existing task/launcher identity, port, OAuth path, receipt/control state, and public routing.
5. One binding at a time.
   Do not fan out recovery across bindings unless each binding independently needs the same runtime refresh.

## Procedure

1. Read the binding's current launcher/task configuration and record its exact backend port and existing OAuth-store path.
2. If the port or OAuth-store identity cannot be determined unambiguously, stop. Do not guess, choose a new port, or repair Caddy/OAuth.
3. Update or restore only the MCP runtime/source/binary for that binding.
4. Restart only that MCP backend using the same task/launcher, the same port, and the same OAuth-store path.
5. Verify the backend directly on its existing loopback port.
6. Verify the already-existing public route without changing the edge.
7. If the backend is healthy on the same port but the public route is not, classify that as a separate edge problem. This recovery procedure still does not touch Caddy.
8. If the OAuth store is missing or invalid, do not manufacture or replace it and do not turn recovery into a re-login flow. Report the auth-state problem separately.

## Forbidden legacy patterns

Do not use old recipes that:
- rebuild or restore Caddy;
- reload Caddy to point at a new backend port;
- stage replacement MCPs on unused ports and cut traffic over;
- restore/copy OAuth files or ask for reauthorization as a normal recovery step;
- restore old topology bundles, recovery-state files, or historical port maps as live truth;
- use regression-research, V3/V5, chat history, incident reports, or archived scripts as MCP recovery authority.

Historical incident material may explain what happened, but it is never an executable recovery recipe.
