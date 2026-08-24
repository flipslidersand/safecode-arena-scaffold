# safecode-arena

Automated evaluation runner for AI-generated code candidates (Rust + Wasmtime).

Executes candidate code inside a WebAssembly sandbox, scores each run against a YAML spec, and produces a ranked comparison report.

## Architecture

```
spec.yaml (test cases + scoring rules)
       │
       ▼
  safecode-arena
       ├─ eval    — sandboxed execution via Wasmtime
       │            records: pass/fail, stdout, runtime, exit code
       ├─ compare — batch eval across a directory of candidates
       │            ranks by pass-rate, performance, and safety
       └─ report  — Markdown summary with per-case breakdown
```

## Usage

```bash
cargo build --release

# Evaluate a single candidate
safecode-arena eval --file candidate.wasm --spec spec.yaml

# Compare multiple candidates in a directory
safecode-arena compare --dir ./candidates --spec spec.yaml --out report.md
```

## Spec format

```yaml
cases:
  - id: hello
    input: "world"
    expected_output: "Hello, world!\n"
    timeout_ms: 500
    max_memory_bytes: 10485760   # 10 MB
```

## Tech Stack

- **Language:** Rust
- **Sandbox:** Wasmtime (component model)
- **Storage:** SQLite (evaluation results via rusqlite)
- **Config:** YAML (serde_yaml)

## Status

Scaffold phase — CLI skeleton complete, evaluation engine in progress.

## License

MIT

