# Legal & Privacy

**License:** MIT (see [LICENSE](../LICENSE) in project root)

**Privacy:**
- Zero telemetry
- All graph data stored locally in `.code-review-graph/graph.db`
- Core parsing and graph storage run locally. Optional providers and integrations can make network calls.
- Optional embeddings model downloaded once from HuggingFace (when using `[embeddings]` extra)

**Data flow:** Google, MiniMax, and remote OpenAI-compatible embedding providers send graph-derived text to the configured service. Local sentence-transformer embeddings run on this machine after downloading the model. MCP tools return data to the connected client; that client may send it to its model provider. Wiki generation renders local graph data into Markdown. Select local providers and review the client configuration when local-only processing is required.

**Warranty:** Provided as-is, without warranty of any kind.
