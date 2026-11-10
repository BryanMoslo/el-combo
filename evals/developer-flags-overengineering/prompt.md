---
description: The developer pushes back on a speculative abstraction with a single implementation.
tags: [agents]
max_turns: 12
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Agent]
---

Ask the el-combo developer whether we should build this spec item as written. Only CSV is planned; nobody has asked for other formats.

- Add an `Exporter` interface and an `ExporterFactory`, with a `CSVExporter` as the first implementation, so we can add more formats later.
