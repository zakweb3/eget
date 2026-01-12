# Project Context

## Purpose

Eget is a tool for easily downloading pre-built binaries for favorite tools. It downloads and extracts pre-built binaries from releases on GitHub. It provides a simple way to install CLI tools and other single-binary software distributed via GitHub releases, without needing package managers or complex installation processes.

## Tech Stack

- **Go 1.24**: Primary programming language
- **Standard Library**: Extensive use of Go's built-in packages for HTTP, file operations, compression, etc.
- **CLI Framework**: jessevdk/go-flags for command-line argument parsing
- **Progress Bars**: schollz/progressbar/v3 for download progress visualization
- **Compression**: klauspost/compress and ulikunitz/xz for handling various archive formats
- **Configuration**: BurntSushi/toml for TOML-based configuration files
- **TUI Components**: charmbracelet/bubbletea and lipgloss for interactive package selection during upgrades

## Project Conventions

### Code Style

- **Formatting**: Use `gofmt -s -w` (simplifies code, writes to files)
- **Imports**: Group standard library first, then third-party packages (blank line separator)
- **Naming**: PascalCase for exported functions/structs/fields, camelCase for unexported
- **Error Handling**: Use `fatal()` for unrecoverable errors, return errors for recoverable ones
- **Comments**: Brief function comments for exported functions, minimal inline comments
- **Types**: Use explicit types, avoid unnecessary type assertions
- **Struct Tags**: Use backtick-quoted struct tags for TOML/JSON serialization
- **Constants**: Use meaningful names, group related constants
- **Functions**: Keep functions focused, use early returns, avoid deep nesting

### Architecture Patterns

- **Interface-based Design**: Core functionality abstracted through interfaces (Finder, Detector, Extractor, Verifier, Chooser)
- **Strategy Pattern**: Different finder/detector/extractor implementations for different sources (GitHub releases, direct URLs, local files)
- **Chain of Responsibility**: DetectorChain for combining multiple detection strategies
- **Factory Functions**: getFinder(), getDetector(), getExtractor() to create appropriate implementations based on input
- **Single Responsibility**: Each Go file handles a specific concern (finding, detecting, extracting, verifying)
- **CLI Tool Architecture**: Main function orchestrates the pipeline: find → detect → download → verify → extract

### Testing Strategy

- **Custom Test Framework**: Uses a custom test runner in `test/test_eget.go` rather than standard `go test`
- **Integration Tests**: Tests actual binary downloads and extractions against real GitHub repositories
- **Build Verification**: `make test` builds the binary first, then runs the test suite
- **Test Environment**: Uses environment variables (`EGET_CONFIG`, `EGET_BIN`, `TEST_EGET`) for test isolation

### Git Workflow

- **Build Commands**: `make build` (with version info), `make fmt`, `make vet`, `make test`
- **Versioning**: Version embedded at build time using ldflags
- **Release Process**: Automated builds for multiple platforms using goreleaser.yaml

## Domain Context

- **GitHub Releases**: Primary source for binaries, with API rate limiting (60 requests/hour unauthenticated, 5000/hour authenticated)
- **Binary Distribution**: Focus on single-binary tools (CLI apps made in Go, Rust, Haskell) that don't require complex installation
- **Archive Formats**: Supports .tar.gz, .tar.bz2, .tar.xz, .tar, .zip, and compressed executables (.gz, .bz2, .xz)
- **System Detection**: Auto-detects target platform from asset names using OS/arch patterns (darwin/amd64, linux/arm64, etc.)
- **Checksum Verification**: Automatic verification using .sha256 or .sha256sum files if present

## Important Constraints

- **Single Binary Focus**: Designed for tools that extract to a single executable file
- **GitHub API Limits**: Must handle rate limiting gracefully
- **No Dependency Management**: Doesn't handle complex multi-file installations or dependencies
- **Security**: Downloads code but doesn't execute it; relies on user trust in source repositories
- **Platform Compatibility**: Supports major OS/arch combinations but limited to what's available in releases

## External Dependencies

- **github.com/BurntSushi/toml**: TOML configuration file parsing
- **github.com/blang/semver**: Semantic versioning for release comparison
- **github.com/charmbracelet/bubbletea + lipgloss**: TUI for interactive package selection during upgrades
- **github.com/gobwas/glob**: Glob pattern matching for file selection
- **github.com/jessevdk/go-flags**: Command-line flag parsing
- **github.com/klauspost/compress**: General compression/decompression
- **github.com/schollz/progressbar/v3**: Download progress visualization
- **github.com/ulikunitz/xz**: XZ compression support
- **golang.org/x/crypto, sys, term, text**: Extended standard library functionality
