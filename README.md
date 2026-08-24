# safecode-arena

Automated evaluation runner for AI-generated code candidates (Rust + Wasmtime).
Executes candidate code inside a WebAssembly sandbox, scores each run against a YAML spec, and produces a ranked comparison report.

AI が生成したコード候補を自動評価するランナーです（Rust + Wasmtime）。
候補コードを WebAssembly サンドボックス内で実行し、YAML スペックに基づいてスコアリングして、ランキング形式の比較レポートを生成します。

## Architecture / アーキテクチャ

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

## Usage / 使い方

```bash
cargo build --release

# Evaluate a single candidate / 単一候補を評価
safecode-arena eval --file candidate.wasm --spec spec.yaml

# Compare multiple candidates / 複数候補を比較
safecode-arena compare --dir ./candidates --spec spec.yaml --out report.md
```

## Spec format / スペックの書き方

```yaml
cases:
  - id: hello
    input: "world"
    expected_output: "Hello, world!\n"
    timeout_ms: 500
    max_memory_bytes: 10485760   # 10 MB
```

## Tech Stack / 技術スタック

- **Language / 言語:** Rust
- **Sandbox / サンドボックス:** Wasmtime (component model)
- **Storage / ストレージ:** SQLite (rusqlite)
- **Config / 設定:** YAML (serde_yaml)

## Status / ステータス

Scaffold phase — CLI skeleton complete, evaluation engine in progress.
スキャフォールドフェーズ — CLI スケルトン完成、評価エンジン実装中。

## License

MIT
