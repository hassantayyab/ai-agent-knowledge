# Choosing models like an engineer: provider, size, context, reasoning (Chapter 2)

## Summary

- Start prototyping with **hosted LLM APIs** (OpenAI/Anthropic/Gemini) to avoid wasting cycles on infra while iterating.
- Model choice is a tradeoff between **accuracy** vs **cost/latency**; start “strong” then optimize cost later.
- **Context window** size can matter a lot; larger windows can enable simpler prototypes (load more context, do less retrieval work up front).
- **Reasoning models** can spend more internal compute before answering; treat them more like “report generators” that need lots of context and examples.
- The book provides a snapshot table of providers/models (May 2025).

## Decision axes

### 1) Hosted vs. open-source

**Recommendation:** begin with hosted APIs even if you expect to end on OSS. You’ll iterate faster and can later switch via a routing layer (model routing library).

### 2) Model size: accuracy vs cost/latency

Bigger models generally:

- cost more
- run slower
- perform better

**Practical default:** use a strong model during prototyping, then downgrade/route for cost once the product behavior is correct.

### 3) Context window size

A larger context window can let you “brute force” early prototypes by stuffing in more data instead of building retrieval/selection logic immediately.

> Example from the book: Gemini Flash 1.5 Pro supports ~2M tokens (roughly ~4,000 pages), enabling ideas like a support assistant with an entire codebase in context.

### 4) Reasoning models

Reasoning models may:

- take longer (seconds/minutes)
- return answers “all at once”
- sometimes stream intermediate “thinking steps”

**Key idea:** you must provide **lots of context** and **good examples** (many-shot prompting). Without that, they “go off the rails.”

## Snapshot: Providers and models (May 2025)

| Provider       | Form factor | Default model  | Cheap/fast model      | Reasoning model(s)                         |
| -------------- | ----------- | -------------- | --------------------- | ------------------------------------------ |
| OpenAI         | Hosted      | GPT-4.1, 4o    | GPT-4.1 mini, 4o-mini | o4-mini, o4-mini-high, o3, o1-pro, o3-mini |
| Anthropic      | Hosted      | sonnet         | haiku                 | None                                       |
| Google Gemini  | Hosted      | gemini-1.5-pro | gemini-1.5-flash      | None                                       |
| Mistral        | OSS         | mistral-next   | mistral-small         | None                                       |
| Meta           | OSS         | llama3-8b      | llama3-2b             | None                                       |
| DeepSeek       | OSS         | deepseek-V2    | None                  | deepseek-r1                                |
| Qwen (Alibaba) | OSS         | Qwen2-72B      | Qwen2-7B, Qwen1.8B    | None                                       |

## Practical takeaways

- Optimize for **iteration speed** first (hosted), and **unit economics** later.
- If you’re unsure, pick:
  - “best” model for correctness during prototyping
  - “cheap/fast” model once outputs are stable
- Treat reasoning models as **structured report writers**: feed them rich context + multiple examples, and expect higher latency.
