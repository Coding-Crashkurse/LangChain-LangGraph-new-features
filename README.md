# New features in LangChain & LangGraph (since v1.0.0)

A code-first Jupyter notebook ([`langchain_langgraph_1x.ipynb`](langchain_langgraph_1x.ipynb)) that demonstrates **only the features added since v1.0.0 (2025-10-20)** with runnable examples. 1.0 basics are assumed known — no migration, no `create_agent` / `StateGraph` intro.

Docs baseline: **LangChain v1.3**, **LangGraph v1.2**. Verified against the installed stack: `langchain==1.3.4`, `langchain-core==1.4.1`, `langgraph==1.2.4`, `langchain-openai==1.2.2`. All demos use **OpenAI**.

## Setup

```bash
uv init --no-package langchain-langgraph-1x
cd langchain-langgraph-1x
uv add langchain langgraph langchain-openai python-dotenv
uv add jupyterlab ipykernel
uv run jupyter lab
```

- API key: put `OPENAI_API_KEY=...` in a `.env` file — the shared cell calls `load_dotenv()` (python-dotenv) to load it.
- Optional (external Jupyter): `uv run python -m ipykernel install --user --name langchain-langgraph-1x`.
- uv docs: https://docs.astral.sh/uv/

## Domain (all dummy)

Throughout: a **pizza-chain branch** + a **customer**. Everything is in-memory — no real DBs/services, checkpointer is `InMemorySaver`. LLM calls are real (`OPENAI_API_KEY`); tools/DB/services are mocks. The shared context (menu, the `get_price` / `place_order` tools, `Customer`, `checkpointer`) is defined once at the top and reused by every demo.

## Features & doc links

The notebook keeps marker cells minimal; the source links live here. Rows are in notebook order.

| # | Feature | Version | Docs |
|---|---------|---------|------|
| 1 | Model profiles (`.profile`) | langchain v1.1 | https://docs.langchain.com/oss/python/langchain/models#model-profiles |
| 2 | Model-retry middleware (`ModelRetryMiddleware`) | langchain v1.1 | https://docs.langchain.com/oss/python/langchain/middleware/built-in#model-retry |
| 3 | Summarization middleware (profile-based trigger) | langchain v1.1 | https://docs.langchain.com/oss/python/langchain/middleware/built-in#summarization |
| 4 | Content-moderation middleware (OpenAI) | langchain v1.1 | https://docs.langchain.com/oss/python/integrations/middleware/openai#content-moderation |
| 5 | Structured output: `ProviderStrategy` + strict schema | langchain v1.1/1.2 | https://docs.langchain.com/oss/python/langchain/structured-output#provider-strategy |
| 6 | Event streaming v3 (`astream_events(version="v3")`; v2 shown first as a baseline) | langchain v1.3 | https://docs.langchain.com/oss/python/langchain/event-streaming |
| 7 | `DeltaChannel` (beta) | langgraph v1.2 | https://docs.langchain.com/oss/python/langgraph/pregel#deltachannel |
| 8 | Per-node timeouts (`TimeoutPolicy`, `NodeTimeoutError`) | langgraph v1.2 | https://docs.langchain.com/oss/python/langgraph/fault-tolerance#timeouts · https://reference.langchain.com/python/langgraph/types/TimeoutPolicy |
| 9 | Node-level error handlers (`error_handler`, `NodeError`, `Command`) | langgraph v1.2 | https://docs.langchain.com/oss/python/langgraph/fault-tolerance#error-handling |
| 10 | Graceful shutdown (`RunControl`, `request_drain()`) | langgraph v1.2 | https://reference.langchain.com/python/langgraph/runtime/RunControl · https://docs.langchain.com/oss/python/releases/changelog |

## Notes

- `DeltaChannel` is **beta** and emits `LangChainBetaWarning`.
- The streaming demo runs `astream_events(version="v2")` first as a baseline, then the new `version="v3"` API for contrast.
- Chat-model objects validate `OPENAI_API_KEY` at construction time, so cells that build a model need the key set; the model-free LangGraph demos (`DeltaChannel`, per-node timeouts, error handlers, graceful shutdown) run without any key.

## References

- Changelog: https://docs.langchain.com/oss/python/releases/changelog
- LangChain v1: https://docs.langchain.com/oss/python/releases/langchain-v1
- LangGraph v1: https://docs.langchain.com/oss/python/releases/langgraph-v1
