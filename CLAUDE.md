# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

goose is a local, extensible, open-source AI agent that automates engineering tasks. It's written in Rust with an Electron-based TypeScript desktop UI. The agent executes tools through the Model Context Protocol (MCP).

## Build & Development Commands

```bash
# Environment setup (required before development)
source bin/activate-hermit

# Build
cargo build                   # debug build
cargo build --release         # release build
just release-binary           # release + OpenAPI generation

# Test
cargo test                    # all tests
cargo test -p goose           # specific crate
cargo test -p goose -- test_name  # single test

# Lint & Format
cargo fmt                     # format code (required before commits)
./scripts/clippy-lint.sh      # full lint suite (required before PRs)
./scripts/clippy-lint.sh --fix  # auto-fix lint issues

# UI Development
just generate-openapi         # regenerate after server route changes
just run-ui                   # build release and start desktop app
just debug-ui                 # connect UI to externally running backend
cd ui/desktop && npm run lint:check && npm run test:run  # UI checks
```

## Architecture

### Crate Structure
- **goose**: Core agent logic, LLM providers, session management, MCP utilities
- **goose-cli**: Command-line interface (entry: `src/main.rs`)
- **goose-server**: REST API backend for desktop app, binary name `goosed` (entry: `src/main.rs`)
- **goose-mcp**: Built-in MCP extensions (developer tools, computer controller, memory)
- **goose-bench**: Benchmarking suite
- **goose-test**: Test utilities

MCP protocol implementation uses the external `rmcp` crate (workspace dependency).

### UI Layer
- Location: `ui/desktop/`
- Framework: Electron + TypeScript + React
- API client auto-generated from OpenAPI schema at `ui/desktop/openapi.json`

### Adding Features
- Non-trivial features: Implement in `goose` crate, call from `goose-cli` or add routes to `goose-server`
- Desktop features: Add routes to `goose-server/src/routes`, then run `just generate-openapi` to update UI API client
- Never edit `ui/desktop/openapi.json` manually

## Code Patterns

### Error Handling
- Use `anyhow::Result` for standard errors
- Use `thiserror::Error` for agent-facing errors that should go back to the LLM
- Avoid `unwrap()` in production code
- Don't add error context that doesn't provide new information

### Code Style
- Write self-documenting code; avoid comments that restate what code does
- Only comment for complex algorithms, non-obvious logic, or "why" not "what"
- Booleans should default to false, not be optional
- Trust Rust's type system; avoid overly defensive code
- Minimize logging; only log errors or security events

### Naming
- In documentation, always refer to the project as "goose" (lowercase), never "Goose"

## Key Entry Points
- CLI: `crates/goose-cli/src/main.rs`
- Server: `crates/goose-server/src/main.rs`
- Agent orchestration: `crates/goose/src/agents/agent.rs`
- UI: `ui/desktop/src/main.ts`
- Providers: `crates/goose/src/providers/`

## Testing
- Place tests in `tests/` folders within each crate
- When adding features, update `goose-self-test.yaml` and run `goose run --recipe goose-self-test.yaml`
- Record MCP integration tests: `just record-mcp-tests`

## CI Requirements
The following must pass before merging:
- `cargo fmt --check`
- `cargo test`
- `./scripts/clippy-lint.sh`
- `just check-openapi-schema`
- UI: `npm run lint:check` and `npm run test:run` (in `ui/desktop/`)
