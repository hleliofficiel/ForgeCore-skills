# AI Workflows & LLM Orchestration

> Distilled engineering knowledge for building reliable, agentic systems and integrating Large Language Models into production software.

Building with AI is no longer about API wrappers. It is about systems engineering. Non-deterministic models must be constrained by deterministic architectures to produce reliable outcomes.

---

# Core Principles

Every AI system should satisfy:

**Determinism via Architecture:** Use architecture, routing, and tool calling to constrain non-deterministic models.
**Evaluation over Intuition:** Do not guess if a prompt works. Build automated evals using specialized datasets.
**Context is King:** The quality of the output is strictly bound by the quality and relevance of the context provided.
**Graceful Degradation:** When the model fails, hallucinates, or times out, the system must fall back gracefully.
**Human in the Loop (HITL):** For high-stakes decisions, models should recommend, and humans should approve.

---

# Retrieval-Augmented Generation (RAG)

RAG grounds the model in factual, domain-specific data.

**Chunking Strategy:** Chunk by semantic meaning (paragraphs, sections) rather than arbitrary character limits.
**Embedding Models:** Match the embedding model to the domain. Use cross-encoders for final re-ranking to improve precision.
**Vector Databases:** Use them for semantic search, but combine with traditional BM25 keyword search for hybrid retrieval. Semantic search often fails on exact keyword matching.
**Metadata Filtering:** Tag chunks with metadata (date, author, category) to pre-filter before running vector similarity. This drastically improves accuracy and performance.

---

# Prompt Engineering as Software Engineering

Treat prompts as source code.

**Version Control:** Prompts must live in Git.
**Structure:** Use XML or Markdown tags to clearly separate instructions, context, and expected output formats.
**Few-Shot Prompting:** Provide examples of inputs and expected outputs. This is the most effective way to steer formatting and tone.
**Chain of Thought (CoT):** Force the model to explain its reasoning before returning the final answer (`<thinking>` tags). This significantly reduces logical errors.

---

# Agentic Patterns

Agents are LLMs equipped with tools and a loop.

**ReAct (Reason + Act):** The foundational pattern. The model reasons about what to do, acts using a tool, observes the result, and repeats until the goal is met.
**Routing:** Use a fast, cheap model to classify the intent of a request and route it to a specialized prompt or sub-agent.
**Tool Calling (Function Calling):** Define strict JSON schemas for tools. The model does not execute code; it outputs a structured request to execute code. The system executes and returns the result.
**State Management:** Agents need memory. Distinguish between short-term memory (the current conversation window) and long-term memory (persisted facts and summaries).

---

# Reliability and Guardrails

LLMs will hallucinate. Engineering must catch it.

**Input Guardrails:** Validate and sanitize user inputs before they reach the model to prevent prompt injection and jailbreaks.
**Output Guardrails:** Validate model outputs before returning them to the user. If requesting JSON, use a parser that can handle minor syntax errors or retry the prompt with the parsing error.
**Self-Correction:** If an output fails validation, feed the error back to the model in an automated retry loop.

---

# AI Operations (LLMOps)

Operating AI in production requires specialized observability.

**Traceability:** Log the exact prompt sent to the model, the exact parameters (temperature, max tokens), and the exact raw response.
**Cost Monitoring:** Track token usage per user, per feature, and per prompt. Models can drain budgets rapidly if uncontrolled.
**Latency:** LLMs are slow. Stream responses to the UI immediately to improve perceived performance.

---

# The ForgeCore AI Standard

A production-ready AI workflow is:
- **Predictable:** It uses structured outputs (JSON) and strict schemas.
- **Measurable:** It has an evaluation suite that runs on every prompt change.
- **Secure:** It assumes the model can be manipulated and sanitizes all inputs and outputs.
- **Focused:** It uses AI to solve specific problems, not as a generic chat interface.

Do not ask the model to do everything. Ask it to do one thing perfectly, and orchestrate the rest with code.
