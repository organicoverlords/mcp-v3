# MCP V3

MCP V3 is the authenticated transport and process-control boundary used by V3 Rust.

Its public tool surface is exactly `start_process`, `read_output`, and `kill_process`.

MCP owns transport, process execution and receipts, bounded output paging, artifact handoff, execution-target delegation, and transport diagnostics. Workflow decisions remain inside the current feature that needs them in V3 Rust; MCP does not grow a generic scheduler, routing registry, recovery framework, or resource-admission service.

MCP does not own Lane Finals, Tiny3D, Vault/Memory, CI fleets, or proof/review policy.

See `V3_CUTOVER.md` and `MCP_V3_CONTRACT.json`.
