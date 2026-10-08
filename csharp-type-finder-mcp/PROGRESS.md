# C# Type Finder MCP Server — Progress & Handoff

**Last updated:** 2026-09-15
**Status:** Task 1 of 8 in progress (~⅓ done)

---

## Goal

An **MCP server over stdio**, written in C#, that answers "where is type `X` defined, and what does it look like?" against a real solution. It resolves types through Roslyn's `MSBuildWorkspace`, so first-party source types resolve — not just compiled metadata.

It must be consumable by the **Kiro agent and any other MCP client** (Claude Desktop, VS Code, Cursor, Claude Code), which means: distributed as a .NET global tool with a stable command name, and zero-configuration solution auto-discovery, since other clients won't pass custom CLI arguments.

## Confirmed requirements

1. Transport: MCP over **stdio**. Stack: .NET / C#.
2. Input: a type name or partial name. Output: fully qualified name, namespace, assembly, type kind, members, source location.
3. Type source: Roslyn `MSBuildWorkspace` opened from a `.sln` or `.csproj`.
4. "Without any errors" means **both**:
   - the server never throws — every failure is a structured tool result (`NotFound`, `InvalidInput`, `WorkspaceLoadFailed`);
   - workspace/compilation diagnostics are reported alongside results, so a degraded load is visible instead of masquerading as "no match".
5. Usable by any MCP client, not just Kiro.

## Research findings (verified 2026-09-15)

- `ModelContextProtocol` **2.2.0** is current. Use this package, **not** `.AspNetCore`, for stdio. Tool surface is attribute-driven: `[McpServerToolType]` + `[McpServerTool]` + `WithToolsFromAssembly()`.
- Roslyn `Microsoft.CodeAnalysis.*` is at **5.9.0**. `MSBuildWorkspace` lives in `Microsoft.CodeAnalysis.Workspaces.MSBuild`.
- **`Microsoft.Build.Locator` must be registered before any MSBuild/Roslyn type is touched.** Most common cause of "works on my machine" failures. Register from a dedicated startup method, guarded by `MSBuildLocator.IsRegistered`, before the host is built.
- **Anything written to stdout corrupts the JSON-RPC stream.** All logging must go to stderr via `LogToStandardErrorThreshold = LogLevel.Trace`.
- Search APIs: `Compilation.GetTypeByMetadataName` (exact fully-qualified), `Compilation.GetSymbolsWithName(predicate, SymbolFilter.Type)` (name/partial), `ISymbol.Locations` (source file/line).
- Local SDK: **dotnet 10.0.400** is the only SDK installed.

### Package set

| Package | Version | Purpose |
|---|---|---|
| `ModelContextProtocol` | 2.2.0 | MCP server + DI/hosting |
| `Microsoft.Extensions.Hosting` | 9.x | host builder |
| `Microsoft.CodeAnalysis.CSharp` | 5.9.0 | symbols, compilations |
| `Microsoft.CodeAnalysis.Workspaces.MSBuild` | 5.9.0 | `MSBuildWorkspace` |
| `Microsoft.Build.Locator` | latest stable | locate the SDK's MSBuild |
| `NUnit` / `Moq` | latest stable | tests |
| `FluentAssertions` | **7.2.0** | assertions (see deviations) |

## Architecture

```
Any MCP Client (Kiro / Claude Desktop / VS Code / Cursor)
        │  stdio JSON-RPC
        ▼
McpServer Host  ──────────────── ILogger → stderr only
        │
        ▼
CSharpTypeTool  [McpServerToolType]
        ├── ITypeNameValidator
        └── ICSharpTypeFinder
                ├── ISolutionPathResolver      (zero-config discovery)
                ├── IRoslynWorkspaceProvider   (MSBuildWorkspace + diagnostics + cache)
                ├── ITypeSymbolSearcher        (GetSymbolsWithName / GetTypeByMetadataName)
                └── ITypeDescriptorMapper      (INamedTypeSymbol → TypeDescriptor)
```

**Layering:** `CSharpTypeFinder.Analysis` holds all Roslyn logic with **no MCP dependency** (fully unit-testable). `CSharpTypeFinder.Mcp` is a thin host + tool adapter.

**Tool surface (2 tools):**
- `find_csharp_type(typeName, solutionPath?, matchMode?, maxResults?, cancellationToken)` → `TypeSearchResponse`
- `list_loaded_projects(solutionPath?)` → project names + load diagnostics, for diagnosing a bad workspace

**Domain model** (one type per file): `TypeSearchRequest`, `TypeSearchResponse`, `TypeSearchStatus` (`Found` / `NotFound` / `InvalidInput` / `WorkspaceLoadFailed`), `TypeNameMatchMode` (`Exact` / `CaseInsensitive` / `Substring`), `TypeDescriptor`, `TypeMemberDescriptor`, `SourceLocationDescriptor`, `WorkspaceDiagnosticDescriptor`.

## Style constraints (non-negotiable)

Load the `code-style-guidelines` skill before writing any C#, and `unit-testing-guidelines` / `unit-test-structure` / `unit-test-mocking` / `unit-test-assertions` before writing tests. Pass them to any sub-agent that generates or reviews code.

- Explicit constructors — **no primary constructors**.
- **Block-bodied methods only** — no expression-bodied members.
- `is null` / `is not null`. Never the null-forgiving `!` in production code.
- `IReadOnlyCollection<T>` + `ToArray()`. Collection expressions `[]`.
- Full names, no abbreviations, anywhere. Descriptive lambda parameter names.
- No explanatory comments. One type per file. Empty types end with `;`.
- Tool/parameter descriptions go in `[Description]` attributes — that is what MCP publishes as the tool schema.

> The official MCP C# SDK samples use expression-bodied methods and static tool classes. **Both violate our guidelines. Deviate from the docs deliberately:** instance tool classes resolved via DI, block bodies.

---

## What is actually done

✅ Created `csharp-type-finder-mcp/` with `CSharpTypeFinder.slnx` (SDK 10 emits the new XML `.slnx` format)
✅ Four projects scaffolded, all targeting `net10.0`:
   - `src/CSharpTypeFinder.Mcp` (console)
   - `src/CSharpTypeFinder.Analysis` (classlib)
   - `tests/CSharpTypeFinder.Mcp.Tests` (NUnit)
   - `tests/CSharpTypeFinder.Analysis.Tests` (NUnit)

## What is NOT done

❌ Projects not yet added to the solution file
❌ No package references added
❌ No code written — default template files (`Program.cs`, `Class1.cs`, `UnitTest1.cs`) still in place and need replacing
❌ Common properties (`Nullable`, `TreatWarningsAsErrors`, `ImplicitUsings`) not set
❌ Tasks 2-8 untouched

### Resume here

```powershell
# 1. add projects to the solution
dotnet sln csharp-type-finder-mcp/CSharpTypeFinder.slnx add `
  csharp-type-finder-mcp/src/CSharpTypeFinder.Mcp/CSharpTypeFinder.Mcp.csproj `
  csharp-type-finder-mcp/src/CSharpTypeFinder.Analysis/CSharpTypeFinder.Analysis.csproj `
  csharp-type-finder-mcp/tests/CSharpTypeFinder.Mcp.Tests/CSharpTypeFinder.Mcp.Tests.csproj `
  csharp-type-finder-mcp/tests/CSharpTypeFinder.Analysis.Tests/CSharpTypeFinder.Analysis.Tests.csproj

# 2. then: package references, project references, Directory.Build.props, ServerInfoTool
```

---

## Open decisions / deviations to confirm

1. **TFM is `net10.0`, not the planned `net9.0`.** Only SDK 10.0.400 is installed locally, so this is the reliable target. Tradeoff: global-tool consumers need the .NET 10 runtime. Switch to `net9.0` if broader reach matters more.
2. **No private test-helper package is available** in this greenfield project, so tests use **plain Moq syntax** rather than wrapper helpers for setup and verification.
3. **FluentAssertions pinned to 7.2.0**, not 8.x — 8.x requires a paid commercial licence.

---

## Task breakdown

| # | Task | Status |
|---|---|---|
| 1 | Scaffold solution + stdio MCP server with `ServerInfoTool` | 🟡 in progress |
| 2 | Result model + input validation (`TypeNameValidator`, `TypeSearchResponse`) | ⬜ |
| 3 | `RoslynWorkspaceProvider` + fixture `SampleLibrary` + `list_loaded_projects` | ⬜ |
| 4 | `TypeSymbolSearcher` + `CSharpTypeFinder` orchestrator | ⬜ |
| 5 | `TypeDescriptorMapper` with full metadata | ⬜ |
| 6 | Harden never-throw contract and cancellation | ⬜ |
| 7 | Zero-configuration solution resolution + options binding | ⬜ |
| 8 | Package as .NET global tool + multi-client README + e2e integration test | ⬜ |

### Task detail

**1. Scaffold + stdio server.** Wire `Host.CreateApplicationBuilder`, `AddMcpServer().WithStdioServerTransport().WithToolsFromAssembly()`, force console logging to stderr. Add instance tool class `ServerInfoTool` exposing `get_server_info` (name/version/configured solution path). Enable `Nullable`, `TreatWarningsAsErrors`, `ImplicitUsings`.
*Tests:* assert the tool assembly registers the expected tool names.
*Demo:* `dotnet run`, connect via MCP Inspector or `McpClient` over `StdioClientTransport`, see `get_server_info` listed and callable.

**2. Result model + validation.** TDD `ITypeNameValidator` / `TypeNameValidator`: reject null/whitespace, over-length, path or invalid identifier characters; accept simple names, fully qualified names, generic arity syntax (`` Dictionary`2 ``). `TypeSearchResponse` / `TypeSearchStatus` with **private constructors and static factory members** (`TypeSearchResponse.InvalidInput(message)`). Register `find_csharp_type` returning `InvalidInput` for now.
*Tests:* table-driven validator cases via `TestCaseSource`; tool returns `InvalidInput` for blank input.

**3. Workspace provider.** `IRoslynWorkspaceProvider` / `RoslynWorkspaceProvider`: register `MSBuildLocator` exactly once before host build (guard with `IsRegistered`), open `.sln` or `.csproj`, subscribe to `WorkspaceFailed` collecting `WorkspaceDiagnosticDescriptor`, cache the loaded `Solution` keyed by absolute path with last-write-time invalidation. Add fixture `tests/Fixtures/SampleLibrary` containing a class, record, interface, enum, generic class, and nested type. Expose `list_loaded_projects`.
*Tests:* lists fixture projects; missing path → `WorkspaceLoadFailed` with diagnostics, no exception.

**4. Search.** `ITypeSymbolSearcher` / `TypeSymbolSearcher` across all projects' compilations: exact via `GetTypeByMetadataName`, otherwise `GetSymbolsWithName` + `SymbolFilter.Type` honouring `TypeNameMatchMode`; dedupe by fully qualified name + assembly; order exact-before-partial then alphabetically; honour `maxResults`. Add `ICSharpTypeFinder` / `CSharpTypeFinder` orchestrating validate → load → search.
*Tests:* exact / case-insensitive / substring matches; unknown → `NotFound`; `maxResults` truncates; ordering asserted.

**5. Metadata mapping.** `ITypeDescriptorMapper` / `TypeDescriptorMapper` from `INamedTypeSymbol`: fully qualified name, namespace, containing assembly, `TypeKind`, accessibility, abstract/sealed/static/record flags, type parameters, base type, interfaces, declared members (`TypeMemberDescriptor`: kind, name, accessibility, signature, static flag), `SourceLocationDescriptor` per declaration (file, line, character), skipping metadata-only locations.
*Tests:* per fixture type shape incl. generic and nested; a metadata-only type yields no source location rather than a null-reference failure.

**6. Never-throw + cancellation.** Wrap the tool boundary: any exception → logged to stderr, converted to a structured failure with `isError` set. Propagate `CancellationToken` through load and search. A single project failing to compile records a diagnostic and the search continues with the rest.
*Tests:* mocked finder that throws → structured error, no propagated exception; pre-cancelled token returns promptly; one broken project still returns matches from healthy ones with diagnostics attached.

**7. Zero-config targeting.** `ISolutionPathResolver` / `SolutionPathResolver`: accept a `.sln` path, `.csproj` path, or directory; given a directory (or nothing) walk **upward** — falling back to the process working directory — for the nearest `.sln`, then fall back to a single `.csproj`. Structured failure when ambiguous (multiple `.sln` at one level) or empty. Bind `TypeFinderOptions` (default solution path, default match mode, default max results, member inclusion toggle) from `appsettings.json`, env vars, and optional `--solution` arg. **Precedence: tool parameter > CLI arg > env var > appsettings.** This is what makes the server work for clients that pass no arguments.
*Tests:* upward-walk discovery from a nested directory; ambiguous-directory failure; `.csproj` fallback; binding precedence.

**8. Global tool + multi-client.** Set `PackAsTool`, `ToolCommandName` = **`csharp-type-finder`**, `PackageId`, package metadata. `dotnet pack`, then verify `dotnet tool install --global --add-source ./nupkg` yields a working command. README registration docs:
- **Kiro** — `.kiro/settings/mcp.json` (workspace) or `~/.kiro/settings/mcp.json` (user); `mcpServers` object with `command`, `args`, `env`, `disabled`, `autoApprove`
- **Claude Desktop** — `claude_desktop_config.json`, `mcpServers` object
- **VS Code** — `.vscode/mcp.json`, `servers` object with `"type": "stdio"`
- **Cursor** — `.cursor/mcp.json`, `mcpServers` object
- **Claude Code** — `claude mcp add csharp-type-finder -- csharp-type-finder`

*Tests:* full stdio round-trip integration test launching the server as a child process via `StdioClientTransport`; assert stdout carries only valid JSON-RPC (no log contamination).

---

## Verification rule for every task

Run `dotnet build` **and** the relevant test project before presenting results. Clean up temporary files. Report anything that could not be verified.
