---
title: Architecture
description: Pipeline stages, interfaces, and data flow.
sidebar:
  order: 2
---

## Pipeline overview

```
IContentSource          Enumerates raw files
       |
       v
LanguageDetector        Extension map + content heuristics
       |
       v
IExtractor              Split large files into practice blocks
       |
       v
Normalizer              Line endings → LF, trailing newline
       |
       v
ContentId               SHA-256(language + normalized code)
       |                First 16 bytes → 32-char hex ID
       v
IMetricCalculator       Lines, characters, symbol density,
       |                max indent depth
       v
LibraryIndexBuilder     Deduplicates, sorts, emits index
       |
       v
ILibraryIndexStore      Serialize/deserialize library.index.json
       |
       v
IContentLibrary         Query by language, source, line count,
                        symbol density range
```

## Key interfaces

Every pipeline stage is behind an abstraction for testability and extensibility:

| Interface | Purpose |
|-----------|---------|
| `IContentSource` | Enumerates raw input files (implement for custom sources) |
| `IExtractor` | Splits large files into right-sized practice blocks |
| `IMetricCalculator` | Computes difficulty metrics per code item |
| `IContentLibrary` | Queries the indexed library by language, difficulty, etc. |
| `ILibraryIndexStore` | Serializes and deserializes the library index |

## Design goals

| Goal | How |
|------|-----|
| **Platform-stable output** | LF normalization, deterministic sort, content-addressed IDs |
| **Zero external dependencies** | Pure .NET 8 BCL — no third-party NuGet packages |
| **Interface-driven** | Every pipeline stage is behind an abstraction |
| **Testable** | xUnit test suite with golden-parity checks |
| **Extensible** | Implement `IContentSource` for custom ingestion, `IExtractor` for custom splitting |

## Content deduplication

Each code item gets a SHA-256 content-addressed ID computed from `language + normalized_code`. The first 16 bytes produce a 32-character hex ID. Importing the same file twice produces one entry — no duplicates accumulate.

## Smart extraction

Large files are automatically split into right-sized practice blocks. Small files are kept whole. Thresholds are configurable via the extractor.
