<!-- OPENSPEC:START -->
# OpenSpec Instructions

These instructions are for AI assistants working in this project.

Always open `@/openspec/AGENTS.md` when the request:
- Mentions planning or proposals (words like proposal, spec, change, plan)
- Introduces new capabilities, breaking changes, architecture shifts, or big performance/security work
- Sounds ambiguous and you need the authoritative spec before coding

Use `@/openspec/AGENTS.md` to learn:
- How to create and apply change proposals
- Spec format and conventions
- Project structure and guidelines

Keep this managed block so 'openspec update' can refresh the instructions.

<!-- OPENSPEC:END -->

# Agent Instructions for eget Repository

## Build Commands
- `make build` - Build the binary with version info
- `make fmt` - Format code with gofmt -s -w
- `make vet` - Run go vet for static analysis
- `make test` - Run tests (builds binary first, then runs custom test runner)

## Test Commands
- Run all tests: `make test`
- Single test: `cd test; EGET_CONFIG=eget.toml EGET_BIN= TEST_EGET=../eget go run test_eget.go` (custom test framework)

## Code Style Guidelines
- **Formatting**: Use `gofmt -s -w` (simplifies code, writes to files)
- **Imports**: Group standard library first, then third-party packages (blank line separator)
- **Naming**: PascalCase for exported functions/structs/fields, camelCase for unexported
- **Error Handling**: Use `fatal()` for unrecoverable errors, return errors for recoverable ones
- **Comments**: Brief function comments for exported functions, minimal inline comments
- **Types**: Use explicit types, avoid unnecessary type assertions
- **Struct Tags**: Use backtick-quoted struct tags for TOML/JSON serialization
- **Constants**: Use meaningful names, group related constants
- **Functions**: Keep functions focused, use early returns, avoid deep nesting
