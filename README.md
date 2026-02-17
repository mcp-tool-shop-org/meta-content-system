# DevOpTyper.Content

> Part of [MCP Tool Shop](https://mcptoolshop.com)


Portable content ingestion, normalization, and indexing for code-typing practice apps.

This library powers the content pipeline behind [Dev-Op-Typer](https://github.com/mcp-tool-shop-org/dev-op-typer) (Windows) and [linux-dev-typer](https://github.com/mcp-tool-shop-org/linux-dev-typer) (cross-platform). It takes source code files in, and produces a normalized, indexed library that both apps can consume identically regardless of platform.

## What It Does

- **Language detection** -- deterministic, rule-based identification from file content and extension
- **Content normalization** -- consistent line endings, whitespace handling, and encoding
- **Code metrics** -- symbol density, line counts, character distribution for difficulty scoring
- **Index generation** -- produces `library.index.json`, a portable catalog of all ingested content
- **Deduplication** -- content-addressed IDs (SHA-256) prevent duplicates across imports

## NuGet Package

| Package | Description |
|---------|-------------|
| `DevOpTyper.Content` | Content ingestion, normalization, language detection, and index generation. Zero external dependencies. |

## Quick Start

```bash
dotnet restore
dotnet test
dotnet run --project src/DevOpTyper.Content.Cli -- --help
```

## Architecture

```
DevOpTyper.Content          Library -- ingestion, normalization, metrics, indexing
DevOpTyper.Content.Cli      CLI -- command-line interface for batch operations
DevOpTyper.Content.Tests    Tests
```

The library exposes interfaces (`IContentLibrary`, `IContentSource`, `IExtractor`, `IMetricCalculator`) so consuming apps can extend or replace any stage of the pipeline.

## Design Goals

- **Platform-stable output.** The same input files produce the same index on Windows, Linux, and macOS.
- **Zero external dependencies.** Pure .NET 8 library -- no NuGet packages beyond the BCL.
- **Interface-driven.** Every stage of the pipeline is behind an abstraction for testability and extensibility.

## License

[MIT](LICENSE)
