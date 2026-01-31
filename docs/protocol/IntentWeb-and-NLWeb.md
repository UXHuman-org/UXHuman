# IntentWeb and NLWeb: Inspiration and Ambition

**Purpose:** Position IntentWeb relative to [NLWeb](https://github.com/nlweb-ai/NLWeb) as a precursor in the same family, and draw concrete inspiration from NLWeb’s protocol and implementation while clarifying how IntentWeb is more ambitious.

---

## 1. What NLWeb Is (Brief)

[NLWeb](https://github.com/nlweb-ai/NLWeb) is an open reference implementation (Python) and protocol for **conversational interfaces to websites**. It:

- Exposes **natural language as the primary interface**: clients send a `query` (and optional context) and receive structured answers.
- Returns responses in **JSON using Schema.org** — a widely adopted vocabulary so both humans and LLMs can interpret results.
- Acts as an **MCP (Model Context Protocol) server** (and plans A2A), with a core `ask` method so the same endpoint serves humans and AI agents.
- Uses **no server-side conversation state**: context is passed in the request (`prev`, `decontextualized_query`).
- Focuses on **information access**: retrieval, summarization, semantic navigation over existing content (products, recipes, events, etc.). Results come from the backend data store, so items are not hallucinated.

In short: **NLWeb is to MCP/A2A what HTML is to HTTP** — a foundational, content-oriented layer for the AI Web.

---

## 2. Same Family, Different Ambition

IntentWeb and NLWeb share the same **direction**: natural language as the primary protocol; one interface for humans and machines; semantic, interpretable responses. They differ in **scope and ambition**.

| Dimension | NLWeb | IntentWeb (UXHuman) |
|-----------|--------|----------------------|
| **Primary goal** | **Access** — answer questions, retrieve and summarize content | **Execution** — fulfill intents (book, cancel, modify, order) |
| **Interaction model** | Stateless request/response; context in request | Stateful, multi-turn; backend-driven incremental resolution |
| **Response semantics** | Schema.org JSON (items, scores, snippets) | Natural language + markdown; optional structured fields |
| **Discovery** | Client knows endpoint (or MCP discovery) | Open discovery via Intent Manifest at well-known path |
| **Trust** | Not specified in the protocol | Attribution chain (required), explicability (optional) |
| **State** | No server-side session | `interaction_id`, flow types, required vs. collected information |

The Architecture Vision already captures it: **NLWeb formalizes access; IntentWeb operationalizes execution.** Where NLWeb connects language to **information**, IntentWeb connects language to **action**.

UXHuman is more ambitious because it aims not only at “ask and get an answer” but at “state an intent and have it **done**” — with verifiable attribution, backend-controlled requirements, and open, permissionless discovery.

---

## 3. What IntentWeb Can Adopt from NLWeb

NLWeb’s protocol and implementation suggest concrete ideas that can strengthen IntentWeb without changing its focus on execution.

### 3.1 Schema.org in Responses

NLWeb returns results as **Schema.org JSON** (`schema_object` per item). IntentWeb uses **markdown** in `message` for semantic structure. The two can coexist:

- **Recommendation:** Allow (or encourage) IntentWeb responses to include an optional **structured payload** (e.g. `results` or `items`) using Schema.org types when the response is informational (e.g. “list of available times” or “product matches”). This gives agents a machine-parseable complement to the markdown narrative and aligns with the same vocabulary NLWeb and many sites already use.
- **Protocol change:** In the IntentWeb spec, add an optional top-level field such as `structured_result` (or `schema_org_items`) for list-like, informational responses, with a reference to Schema.org (e.g. `ItemList`, `Product`, `Event`). Markdown remains the primary human/LLM-facing content.

### 3.2 MCP and A2A Alignment

NLWeb exposes an **MCP server** (`/mcp`) and plans A2A. IntentWeb does not currently define MCP or A2A bindings.

- **Recommendation:** Define how an IntentSite can **also** expose MCP tools (e.g. “ask” for informational intents, “intent” for execution) or how an MCP/A2A client discovers and calls IntentWeb endpoints. This could be a separate “IntentWeb over MCP” note: e.g. one MCP tool per capability in the Intent Manifest, with request/response mapped to the IntentWeb envelope. That way, agents that only speak MCP today can still participate in the intent economy.

### 3.3 Simple “Ask” Pattern for Informational Intents

NLWeb’s **stateless** `/ask` with `query` (+ optional `prev`, `decontextualized_query`) is a good fit for **informational** intents: “Do you have vegan options?”, “What are your opening hours?”. IntentWeb’s **stateful** flow is essential for **transactional** intents (book, cancel, pay).

- **Recommendation:** In the Intent Manifest, allow capabilities to be tagged as **informational** vs **transactional**. For informational-only sites (or informational capabilities), IntentSites could expose an **optional** NLWeb-style endpoint (e.g. `GET/POST /ask`) with a single `query` and a Schema.org-ish response, while keeping `/intent` for multi-turn execution. The protocol doc could explicitly say: “For informational-only interactions, an IntentSite MAY expose a simpler, stateless ask endpoint; the IntentWeb envelope is required for execution.”

### 3.4 Request Identifiers and Modes

NLWeb uses **`query_id`** (auto-generated if absent) and **`mode`** (`list` | `summarize` | `generate`) to control result shape. IntentWeb has **`interaction_id`** and **`flow_type`**.

- **Inspiration:** The idea of a **mode** (list vs summarize vs generate) could map to IntentWeb as optional **response preferences** (e.g. “return only list of options” vs “return list + natural language summary”) in the request envelope, so agents can request minimal or rich responses. This could be an optional field (e.g. `response_preference`) in a future revision.

### 3.5 REST API Clarity

NLWeb’s [REST API](https://github.com/nlweb-ai/NLWeb/blob/main/docs/nlweb-rest-api.md) is minimal and clear: required/optional args, response shape, streaming option.

- **Inspiration:** Keep the IntentWeb spec equally explicit: required vs optional fields, one place for request and response envelopes, and a single “Life of an Intent” or “Request/Response flow” section (similar to NLWeb’s “Life of a Chat Query”) so implementers see the full path from discovery to execution.

### 3.6 No Hallucination of Items

NLWeb stresses that **returned items come from the database**, so results may be suboptimal but are not invented. IntentWeb’s §7 already says: data in `message` **MUST** come from authoritative systems; the semantic layer only formats.

- **Inspiration:** Make this even more visible in the protocol and critique: “Facts from backend only” is a shared principle with NLWeb; IntentWeb extends it to **execution** (e.g. available times, booking IDs) as well as informational content.

---

## 4. Coexistence: One Site, Two Layers

A single site can offer **both** NLWeb-style access and IntentWeb-style execution:

| Layer | Endpoint (example) | Protocol | Use case |
|-------|--------------------|----------|----------|
| **Information** | `/ask` or MCP `ask` | NLWeb-like: `query` → Schema.org results | “What’s on the menu?”, “Do you have gluten-free options?” |
| **Execution** | `/intent` | IntentWeb: envelope, flow types, attribution | “Book a table for 2 tomorrow at 7pm”, “Cancel reservation RES-123” |

Discovery could be unified: the **Intent Manifest** lists both capabilities — e.g. “Answer questions about menu and hours” (informational, NLWeb-style or simple GET) and “Book, cancel, modify reservations” (transactional, IntentWeb). AI gateways that understand both can route to `/ask` vs `/intent` by capability type.

---

## 5. Summary

- **NLWeb** is a concrete, working proxy for what UXHuman aims at in the **information** space: natural language as protocol, same API for humans and agents, Schema.org, MCP.
- **IntentWeb** goes further: **execution** (stateful, backend-driven, with attribution and open discovery), while staying compatible in spirit with NLWeb.
- **Adopting from NLWeb:** optional Schema.org in responses, MCP/A2A alignment, optional stateless “ask” for informational intents, clear REST-style spec and “life of an intent” flow, and reinforcing “no hallucination of facts.”
- **Coexistence:** one site can expose both an NLWeb-style ask endpoint and an IntentWeb intent endpoint, with a single Intent Manifest describing informational and transactional capabilities.

This document can be used to align the IntentWeb protocol with NLWeb where it helps (Schema.org, MCP, ask pattern) and to explain to implementers and the community why IntentWeb is more ambitious and how the two fit together.

---

**References**

- [NLWeb (GitHub)](https://github.com/nlweb-ai/NLWeb) — main reference implementation (Python).
- [NLWeb REST API](https://github.com/nlweb-ai/NLWeb/blob/main/docs/nlweb-rest-api.md) — `/ask` and `/mcp`, request/response shape.
- [Life of a Chat Query](https://github.com/nlweb-ai/NLWeb/blob/main/docs/life-of-a-chat-query.md) — query processing, tools, Schema.org, no hallucination of items.
- [The IntentWeb Architecture Vision](../architecture/The-IntentWeb-Architecture-Vision.md) — §3.1 comparison: NLWeb formalizes access, IntentWeb operationalizes execution.

© 2025 UXHuman Initiative / Smart Goldfish SAS  
Same license as the IntentWeb Protocol specification.
