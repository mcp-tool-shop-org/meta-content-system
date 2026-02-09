# DevOpTyper.Content (shared core library)

Portable, local-first content ingestion + normalization + indexing for:
- Dev-Op-Typer (Windows)
- linux-dev-typer (Linux)

## Quick start
```bash
dotnet restore
dotnet test
dotnet run --project src/DevOpTyper.Content.Cli -- --help
```

## Core outputs
- `library.index.json` (portable across OSes)
- Deterministic language detection, normalization, and metrics

See `docs/` for the parity contract.
