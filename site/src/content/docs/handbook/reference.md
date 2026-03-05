---
title: Reference
description: CLI commands, metrics, and supported languages.
sidebar:
  order: 3
---

## CLI commands

| Command | Description |
|---------|-------------|
| `build --source <dir> --out <file>` | Build a library index from source files |
| `paste --lang <lang> --title <title> --text <text>` | Add a single snippet to the index |

## Metrics computed

| Metric | Type | Description |
|--------|------|-------------|
| `Lines` | int | Total line count |
| `Characters` | int | Total character count including whitespace |
| `SymbolDensity` | float | Ratio of symbol characters to non-whitespace (0.0-1.0) |
| `MaxIndentDepth` | int | Deepest indentation level (4-space tabs) |

These metrics power adaptive difficulty in consuming apps. A snippet with high symbol density and deep nesting is harder to type than flat prose.

## Supported languages

The language detector covers 20+ languages via extension mapping and content heuristics:

Python, C#, Java, JavaScript, TypeScript, SQL, Bash, Rust, Go, Kotlin, C, C++, JSON, YAML, Markdown — and more via extension map. Unknown files fall back to `text`.

## Query API

```csharp
var library = new InMemoryContentLibrary(index.Items);
var results = library.Query(new ContentQuery
{
    Language = "python",       // filter by language
    MinLines = 5,              // minimum line count
    MaxSymbolDensity = 0.4f    // maximum symbol density
});
```

## Consuming apps

| App | Platform | Repository |
|-----|----------|------------|
| Dev-Op-Typer | Windows (WinUI 3) | mcp-tool-shop-org/dev-op-typer |
| linux-dev-typer | Cross-platform (.NET) | mcp-tool-shop-org/linux-dev-typer |

## Project structure

```
meta-content-system/
├── src/
│   ├── DevOpTyper.Content/        # Core library (NuGet package)
│   └── DevOpTyper.Content.Cli/    # CLI tool for batch indexing
├── tests/
│   └── DevOpTyper.Content.Tests/  # xUnit tests
├── docs/                          # Design documents
└── spec/                          # JSON schemas
```
