# learning

Personal workspace — side projects and structured learning notes.

## Projects

### [`csharp-type-finder-mcp/`](csharp-type-finder-mcp/)

An MCP (Model Context Protocol) server over stdio, written in C# / .NET 10, that answers
*"where is type `X` defined, and what does it look like?"* against a real solution.

It resolves types through Roslyn's `MSBuildWorkspace`, so first-party source types resolve — not just
compiled metadata — and returns the fully qualified name, namespace, assembly, type kind, members and
source location.

Design notes worth calling out:

- **stdio transport**, so anything written to stdout corrupts the JSON-RPC stream. All logging goes to stderr.
- **Never-throw tool contract.** Every failure comes back as a structured result (`NotFound`,
  `InvalidInput`, `WorkspaceLoadFailed`) with workspace diagnostics attached, because "no match found"
  and "your solution failed to load" are very different answers.
- **Zero-configuration solution discovery**, so clients that pass no CLI arguments still work —
  it walks upward for the nearest `.sln`, falling back to a single `.csproj`.
- Packaged as a **.NET global tool** for use from any MCP client (Claude Desktop, VS Code, Cursor, Kiro).

Status: in progress. See [`PROGRESS.md`](csharp-type-finder-mcp/PROGRESS.md) for the task breakdown
and current state.

### [`ANGULAR_LEARNING_CONTEXT.md`](ANGULAR_LEARNING_CONTEXT.md)

A running log of a structured Angular / TypeScript learning track — concepts covered, diagnostics,
corrections, and an 8-week roadmap ending in a full-stack capstone on an ASP.NET Core API. Written as
a durable handoff document rather than a tutorial summary.

## Stack

C#, .NET 10, Roslyn, Model Context Protocol, NUnit, Moq · TypeScript, Angular
