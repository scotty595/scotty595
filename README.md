# Scotty 👋

**Co-Founder & CTO @ [Louvain](https://louvain.ai)**

I build Louvain's knowledge core: a permission-aware, bi-temporal company knowledge graph, with hybrid retrieval and a verifying reader on top. It sits behind Louvain's product and its MCP server for AI agents.

**Open source:** [**louvain-ai/louvain-engine**](https://github.com/louvain-ai/louvain-engine): permission-aware hybrid retrieval for enterprise AI, with a permission check in every query, from one command. Also [claude-plugin](https://github.com/louvain-ai/claude-plugin) and [cursor-plugin](https://github.com/louvain-ai/cursor-plugin).

**Open models on 🤗 Hugging Face** (Apache-2.0):
- [**Louvain/embed-4b-erb**](https://hf.co/Louvain/embed-4b-erb): Qwen3-Embedding-4B fine-tuned for enterprise retrieval (Matryoshka, sentence-transformers).
- [**Louvain/rerank-4b-erb**](https://hf.co/Louvain/rerank-4b-erb): Qwen3-Reranker-4B fine-tuned as the cross-encoder reranker.

---

## 🧠 What's under the hood

**Knowledge core**
- **Bi-temporal knowledge graph.** Every statement carries *valid time* (`asOf`) and *transaction time* (`knownAt`), so the graph can answer "what was true then" and "what did we know then" separately. Rollback covers statements, retractions, verdicts and pipeline runs.
- **Append-only statements with a single writer.** Pipelines propose; one commit path validates declared writes against source spans and stamps provenance. Polarity, modality, certainty and time are first-class.
- **State as a fold.** Current state is a per-viewer fold of admitted statements. Effects declare which events open and close which states; nothing is inferred implicitly.
- **Identity by identifiers or explicit decisions only.** No merging on display names. `same_as` / `different_from` are recorded decisions with retraction.

**Permissions**
- **Zanzibar-style ReBAC on SpiceDB.** The fence runs *before* every read: hidden evidence can't contribute a value, a count, a graph node or a trace stage.
- **Outbox → relay → projection.** Permission changes are delivered durably, and the readable-set projection is narrowed inside the request's own transaction, so a revocation is in force when the request returns.

**Retrieval and reading**
- **Hybrid search.** BM25 lexical plus dense vectors (pgvector), fused with Reciprocal Rank Fusion, then a cross-encoder rerank ([rerank-4b-erb](https://hf.co/Louvain/rerank-4b-erb)).
- **Embeddings.** A fine-tuned Qwen3 embedder ([embed-4b-erb](https://hf.co/Louvain/embed-4b-erb), Matryoshka) served from Python sidecars, kept off the API's event loop, with a bulk lane for ingest.
- **A verifying reader.** Every figure, date and name in an answer is checked sentence by sentence against the passage it came from; unsupported sentences are dropped or flagged. Arithmetic (totals, counts, breakdowns) is computed over the fenced fold, never by the model.
- **An agent loop with forced reads, answer modes and source arbitration,** benchmarked on EnterpriseRAG-Bench with an LLM-judge harness.

**Ingest and infrastructure**
- **Connectors.** Brokered OAuth across Google Workspace, Microsoft 365, Slack, Notion and more. Leased runners, cursors that advance only after acceptance, and source deletions propagated as withdrawals.
- **Queue.** `event_log` is the source of truth and the broker only dispatches. A step ledger keyed on inputs and vocabulary, so work is never paid for twice. Per-credential spend caps and a model-call governor.
- **Runtime.** TypeScript (strict) on Node, Postgres, SpiceDB, Python NLP sidecars, a Tauri desktop shell, Fly.io and Vercel. A self-hosted build and test pipeline.

---

## 🛠️ Stack

```
TypeScript (strict) • Node.js • Fastify • React • Next.js • Rust • Tauri • Python
PostgreSQL • pgvector • BM25 • SpiceDB (Zanzibar ReBAC) • pg-boss
Qwen3 (fine-tuned embed + rerank) • sentence-transformers • RRF • LLM evaluation harnesses
MCP • OAuth 2.1 / DCR • Fly.io • Vercel • Docker • Turborepo • Biome
```

---

## 💼 Previously

- 🎯 **CPO** @ Lua AI (YC F25): agent runtime, memory and governance
- 🌳 **Founder** @ DriftOS: conversation graph engine (patent-pending)
- 📈 **Acting Head of Product** @ Antler: scaled Launch Academy to 10,000+ users
- 🏛️ **Product Manager** @ Department of Justice, Queensland
- 💻 **Ex-IBM**

---

📍 London · [louvain.ai](https://louvain.ai) · [louvain-engine](https://github.com/louvain-ai/louvain-engine) · [LinkedIn](https://linkedin.com/in/shipsfast) · scott@louvain.ai
