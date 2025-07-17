# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Gemini CLI** is a command-line AI workflow tool that connects to Google's Gemini API, providing intelligent code assistance, file operations, and system integration capabilities. It's built with TypeScript and uses a modular architecture with two main packages.

## Architecture

The project follows a two-tier architecture:

- **CLI Package (`packages/cli/`)**: React-based terminal UI using Ink, handles user interaction, theming, and display
- **Core Package (`packages/core/`)**: Backend logic for Gemini API communication, tool orchestration, and state management

Key components:
- **Tools System**: Extensible tools for file operations, shell commands, web search, memory management, and MCP server integration
- **Sandboxing**: Optional containerized execution (Docker/Podman) for security
- **Authentication**: Google OAuth2, API keys, or Vertex AI integration
- **Telemetry**: OpenTelemetry-based metrics and logging

## Development Commands

### Setup & Build
```bash
npm install                    # Install dependencies
npm run build                 # Build main project
npm run build:all            # Build main + sandbox container
npm run clean                # Remove generated files
```

### Development
```bash
npm start                    # Run CLI from source
npm run debug               # Debug mode with inspector
npm run dev                 # Development with React DevTools
```

### Testing
```bash
npm run test                # Unit tests for all packages
npm run test:ci             # Tests with coverage
npm run test:e2e            # Integration tests
npm run test:integration:all  # All integration test variants
```

### Code Quality
```bash
npm run lint                # ESLint check
npm run lint:fix            # Auto-fix linting issues
npm run format              # Prettier formatting
npm run typecheck           # TypeScript type checking
npm run preflight           # Full quality check (format + lint + build + test)
```

### Package-Specific Commands
```bash
# CLI package
cd packages/cli && npm run test     # CLI-specific tests
cd packages/cli && npm run build    # Build CLI only

# Core package  
cd packages/core && npm run test    # Core-specific tests
cd packages/core && npm run build   # Build core only
```

## Key File Locations

- **Entry Points**: `packages/cli/index.ts`, `packages/core/index.ts`
- **Tools**: `packages/core/src/tools/` - All available tools (file ops, shell, search, etc.)
- **UI Components**: `packages/cli/src/ui/` - React components for terminal interface
- **Configuration**: `packages/cli/src/config/` - Auth, settings, sandbox config
- **Tests**: `packages/*/src/**/*.test.ts` - Unit tests alongside source
- **Integration**: `integration-tests/` - End-to-end test suite

## Development Workflow

1. **Prerequisites**: Node.js 20+ (use 20.19.0 for development)
2. **Setup**: `npm install && npm run build`
3. **Development**: `npm start` or `npm run debug`
4. **Testing**: `npm run preflight` before commits
5. **Debugging**: Use VS Code F5 or `npm run debug` with Chrome inspector

## Authentication Setup

For development, set up authentication:
- **Google OAuth**: Run CLI and authenticate interactively
- **API Key**: `export GEMINI_API_KEY="your-key"`
- **Vertex AI**: `export GOOGLE_API_KEY="your-key"` + `export GOOGLE_GENAI_USE_VERTEXAI=true`

## Sandboxing

Optional security sandboxing:
- **Container**: `export GEMINI_SANDBOX=true` (uses Docker/Podman)
- **MacOS**: Uses Seatbelt profiles by default
- **Custom**: Configure via `.env` or `.gemini/` directory settings

## MCP Integration

The CLI supports Model Context Protocol (MCP) servers:
- MCP tools located in `packages/core/src/tools/mcp-*`
- Configuration via MCP client/server setup
- Extensible tool registry system