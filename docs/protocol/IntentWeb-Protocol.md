# IntentWeb Protocol Specification

**Version:** 1.0-draft  
**Status:** Draft for community review  
**Aligns with:** [The IntentWeb Architecture Vision](../architecture/The-IntentWeb-Architecture-Vision.md)

---

## 1. Overview

This document specifies the **IntentWeb Protocol**—a concrete protocol for intent-based, natural-language communication between users, AI actors, and companies (IntentSites). The protocol realizes the six core concepts and four foundational values described in the IntentWeb Architecture Vision.

### 1.1 Protocol Goals

| Goal | How the protocol achieves it |
|------|------------------------------|
| **Natural language as primary protocol** | Intent and responses are expressed in natural language; structured fields are for routing and state only. |
| **Trust through verification** | Every message includes a required attribution chain; optional explicability explains decisions. |
| **Minimal mental load** | Incremental resolution, explicit flow types, and markdown responses reduce cognitive effort. |
| **Sovereignty** | Companies publish manifests and own execution; users and agents own presentation and consent. |

### 1.2 Roles

- **User:** Human or entity expressing intent.
- **AI Agent:** Interprets user intent and may send/receive protocol messages on their behalf.
- **AI Gateway:** Discovers IntentSites, routes intent, and may aggregate or present results.
- **IntentSite:** Company system that discovers capabilities via a manifest and fulfills intent via the intent endpoint.

### 1.3 Conventions

- **MUST** / **MUST NOT:** Required for protocol compliance.
- **SHOULD** / **SHOULD NOT:** Recommended for interoperability and trust.
- **MAY:** Optional.

---

## 2. Discovery: Intent Manifest

*Realizes: Open Discovery (Concept 6), Sovereignty*

### 2.1 Well-Known Location

An IntentSite **MUST** publish an Intent Manifest at a well-known URL:

```
{origin}/intentmanifest.yaml
```

Example: `https://bellacucina.com/intentmanifest.yaml`

Clients (AI gateways, crawlers, agents) **MAY** discover manifests by:

- Crawling this well-known path per origin.
- Following links from other resources (e.g., `Link` header or HTML).

### 2.2 Manifest Format

The manifest **MUST** be valid YAML. Content **MUST** be UTF-8.

**Required top-level fields:**

| Field | Type | Description |
|-------|------|-------------|
| `manifest_version` | string | Protocol version the manifest conforms to (e.g. `"1.0"`). |
| `company` | string | Human-readable name of the company or service. |
| `last_updated` | string | Date of last meaningful change (ISO 8601 date or date-time). |
| `capabilities` | array | List of capability objects (see below). |
| `contact` | object | At least `intent_endpoint` (see below). |

**Optional top-level fields:**

| Field | Type | Description |
|-------|------|-------------|
| `about` | string | Short natural-language description of the company/service. |
| `schema_type` | string | Schema.org type URL (e.g. `https://schema.org/Restaurant`). |
| `location` | object | Address, region, service type, etc., in natural language or structured. |

**Capability object (each item in `capabilities`):**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `intent` | string | Yes | Natural-language summary of the intent (e.g. "Book a table for dining"). |
| `description` | string | Yes | Short explanation of what this capability does. |
| `examples` | array of strings | Recommended | Example user utterances. |
| `requires` | array of strings | Recommended | Natural-language list of required information. |
| `constraints` | array of strings | Optional | Business rules, limits, timing. |
| `notes` | array of strings | Optional | Additional information for agents or users. |
| `operating_hours` | object | Optional | Keys and values in natural language. |

**Contact object:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `intent_endpoint` | string (URL) | Yes | Base URL for intent API (e.g. `https://bellacucina.com/intent`). |
| `website` | string (URL) | Optional | Human-facing website. |
| `phone` | string | Optional | Contact phone. |
| `email` | string | Optional | Contact email. |

All narrative content (intent, description, requires, constraints, etc.) **MUST** be in natural language so that both humans and language models can interpret it without a separate schema.

### 2.3 Manifest Example (Minimal)

```yaml
manifest_version: "1.0"
company: "Bella Cucina Restaurant"
last_updated: "2025-10-15"
capabilities:
  - intent: "Book a table for dining"
    description: "Reserve a table at our restaurant for lunch or dinner"
    examples:
      - "Book a table for 2 people tomorrow at 7pm"
    requires:
      - "Number of people in your party (we accommodate 1-20)"
      - "Guest name for the reservation"
      - "Preferred date"
      - "Preferred time"
contact:
  intent_endpoint: "https://bellacucina.com/intent"
  website: "https://bellacucina.com"
```

---

## 3. Intent Endpoint

*Realizes: Natural Language as Protocol (Concept 1), Incremental Intent Resolution (Concept 4)*

### 3.1 Endpoints

| Purpose | Method | Path | Use case |
|---------|--------|------|----------|
| Intent API | POST | `{intent_endpoint}` | Machine-to-machine (agents, gateways). |
| Intent UI | GET | `{origin}/intent-ui/` | Human in browser; same logical flow, presentation by IntentSite. |

The Intent API path is the `intent_endpoint` from the manifest (e.g. `https://bellacucina.com/intent`). The same backend logic **SHOULD** drive both API and UI so that human and machine interactions are equivalent.

### 3.2 Request Envelope (POST to Intent Endpoint)

**Content-Type:** `application/json`

**Required fields:**

| Field | Type | Description |
|-------|------|-------------|
| `protocol_version` | string | IntentWeb protocol version (e.g. `"1.0"`). |
| `flow_type` | string | One of the flow types in § 3.4. |
| `message` | string | Natural-language content (user intent, answer, or system message). |
| `interaction_id` | string | Opaque id for this conversation/session; client-generated, stable for the whole interaction. |
| `attribution` | object | Chain of custody; see § 5. |

**Optional but recommended:**

| Field | Type | Description |
|-------|------|-------------|
| `explicability` | array | Optional reasoning; see § 6. |
| `locale` | string | BCP 47 language tag for message language. |
| `timestamp` | string | ISO 8601 time when the sender created the message. |

**Example: Initiate intent**

```json
{
  "protocol_version": "1.0",
  "flow_type": "intent_request",
  "message": "Book a table for 2 people under Jane Smith on October 15 at 7pm. Window seat if possible.",
  "interaction_id": "conv-abc123",
  "attribution": { ... },
  "timestamp": "2025-10-15T19:23:41Z"
}
```

### 3.3 Response Envelope

**Content-Type:** `application/json`

**Required fields:**

| Field | Type | Description |
|-------|------|-------------|
| `protocol_version` | string | Same as request. |
| `flow_type` | string | One of the flow types in § 3.4. |
| `message` | string | Natural-language and/or markdown content (§ 4). |
| `interaction_id` | string | Echo of the request’s `interaction_id`. |
| `attribution` | object | IntentSite (and optionally gateway) attribution; see § 5. |

**Conditional / optional:**

| Field | Type | When | Description |
|-------|------|------|-------------|
| `required_information` | array of strings | When asking for more info | Natural-language list of what is still needed. |
| `collected_information` | object | Optional | Key-value summary of what has been collected (for debugging/transparency). |
| `status` | string | For `execution_result` or `error` | e.g. `confirmed`, `failed`, `invalid_request`. |
| `external_id` | string | For `execution_result` | Booking ID, order ID, etc. |
| `explicability` | array | Optional | See § 6. |
| `timestamp` | string | Optional | ISO 8601. |

### 3.4 Flow Types

Flow types define the **state** of the interaction. They are mandatory and machine-readable.

| Flow type | Direction | Meaning |
|-----------|-----------|---------|
| `intent_request` | Client → IntentSite | Start a new intent (e.g. "Book a table for 2"). |
| `information_request` | Either | Request more information (e.g. "What date would you like?" or "Do you have vegan options?"). |
| `information_response` | Either | Provide requested information (e.g. "Tomorrow at 7pm" or "Yes, we have vegan options."). |
| `clarification_request` | Either | Ask for disambiguation (e.g. "Did you mean Chicago or Chicago Heights?"). |
| `execution_result` | IntentSite → Client | Intent completed (e.g. "Booking confirmed: RES-12345"). |
| `error` | Either | Unrecoverable error; interaction for this intent should stop or reset. |

**Rules:**

- The first message in a new intent **MUST** be `intent_request`.
- `execution_result` and `error` are terminal for that intent; a new intent requires a new `interaction_id` (or equivalent new conversation).
- `required_information` **SHOULD** be set on `information_request` so agents know what to collect next.

---

## 4. Message Content: Markdown as Semantic Structure

*Realizes: Markdown as Semantic Structure (Concept 2), Adaptive Presentation (Concept 3)*

### 4.1 Format of `message`

The `message` field **MUST** be a string. It **MAY** be:

- Plain natural language, or
- Markdown (CommonMark) for structure (headings, tables, lists, emphasis, links).

IntentSites **SHOULD** use markdown to express:

- Lists of options (tables, lists).
- Emphasis (e.g. **Best value**, *Note*).
- Links to deeper content or actions (e.g. `[View all products](https://company.com/products)`).

Consumers (agents, gateways, UIs) **MAY** interpret markdown for their context (voice, GUI, terminal). The protocol does not prescribe presentation; it only specifies that meaning is conveyed in natural language and markdown.

### 4.2 Example Response (Information Request with Table)

```json
{
  "protocol_version": "1.0",
  "flow_type": "information_request",
  "message": "I have these times available on October 16:\n\n| Time  | Availability |\n|-------|--------------|\n| 18:00 | Available    |\n| 18:30 | Available    |\n| 19:00 | Available    |\n| 19:30 | Limited      |\n\nWhich time works for you?",
  "interaction_id": "conv-abc123",
  "required_information": ["time"],
  "attribution": { ... }
}
```

---

## 5. Attribution (Required)

*Realizes: Trust Through Attribution & Explicability (Concept 5)*

Every request and response **MUST** include an `attribution` object. Attribution provides a verifiable chain of custody and supports non-repudiation.

### 5.1 Attribution Object

**Required:**

| Field | Type | Description |
|-------|------|-------------|
| `query_hash` | string | Hash of the initial intent payload (or canonical representation) to detect tampering. |
| `nonce` | string | Unique value for this request/response to prevent replay. |
| `timestamp` | string | ISO 8601 time of this message. |
| `chain` | array | Ordered list of actors who have handled this intent (see below). |

**Chain entry (each item in `chain`):**

| Field | Type | Description |
|-------|------|-------------|
| `actor_type` | string | e.g. `ai_agent`, `ai_gateway`, `intent_site`. |
| `actor_id` | string | Stable identifier for this actor (e.g. endpoint URL, agent id). |
| `timestamp` | string | ISO 8601 when this actor processed the message. |
| `signature` | string | Cryptographic signature over (e.g.) query_hash + nonce + timestamp + actor_id; algorithm and encoding (e.g. base64) **SHOULD** be documented or indicated. |

IntentSites **MUST** verify the chain (signatures and optional certificate/identity checks) to the extent they need to trust the sender. Clients **MAY** verify IntentSite signatures in responses.

### 5.2 Example Attribution (Request from Agent via Gateway)

```json
"attribution": {
  "query_hash": "a1b2c3d4e5f6...",
  "nonce": "unique-12345",
  "timestamp": "2025-10-15T19:23:41Z",
  "chain": [
    {
      "actor_type": "ai_agent",
      "actor_id": "personal-assistant-v2",
      "timestamp": "2025-10-15T19:23:41Z",
      "signature": "sig_agent_abc..."
    },
    {
      "actor_type": "ai_gateway",
      "actor_id": "https://gateway.example.com",
      "timestamp": "2025-10-15T19:23:42Z",
      "signature": "sig_gateway_def..."
    }
  ]
}
```

---

## 6. Explicability (Optional)

*Realizes: Trust Through Attribution & Explicability (Concept 5)*

Explicability explains **why** decisions were made. It is **optional** but **SHOULD** be included when useful for users or compliance.

### 6.1 Explicability Array

Each element typically represents one actor’s reasoning:

| Field | Type | Description |
|-------|------|-------------|
| `actor` | string | Same as `actor_type` or `actor_id` in attribution. |
| `interpretation` | string | How the actor understood the intent or message. |
| `assumptions` | array of strings | Optional assumptions made. |
| `confidence` | number | Optional 0–1 confidence. |
| `routing_logic` | string | Optional (e.g. for gateways): why this IntentSite was chosen. |
| `sites_considered` | array of strings | Optional list of IntentSites considered. |

### 6.2 Example

```json
"explicability": [
  {
    "actor": "ai_agent",
    "interpretation": "User wants to book a table for 2 in Chicago tomorrow",
    "assumptions": ["dinner", "Chicago downtown"],
    "confidence": 0.9
  },
  {
    "actor": "ai_gateway",
    "routing_logic": "Selected restaurants with table booking and Chicago location",
    "sites_considered": ["bellacucina.com", "other-restaurant.com"]
  }
]
```

---

## 7. Incremental Intent Resolution (Behavioral Contract)

*Realizes: Incremental Intent Resolution (Concept 4)*

The protocol does not fix a single state machine, but the following **SHOULD** hold:

1. **Requirements from backend:** What information is required (e.g. party size, date, time) is determined by the IntentSite’s backend/business logic, not by the client.
2. **Progress:** The IntentSite tracks what has been collected and what is still missing and sends `information_request` with `required_information` until sufficient to execute.
3. **Execution:** Only when all required information is collected and validated does the IntentSite send `execution_result` (or `error`).
4. **Facts from backend:** Data in `message` (e.g. available times, prices) **MUST** come from authoritative systems; the semantic layer (e.g. LLM) may only format, not invent, facts.

Clients **SHOULD** use `interaction_id` to keep all turns of one intent in a single conversation and **SHOULD** send `information_response` or `clarification_request`/response as appropriate until `execution_result` or `error`.

---

## 8. Errors

When something unrecoverable happens, the IntentSite (or client) **SHOULD** respond with `flow_type: "error"` and set:

- `message`: Human- and machine-readable explanation.
- `status`: Optional code or label (e.g. `invalid_request`, `backend_unavailable`, `rate_limited`).

Example:

```json
{
  "protocol_version": "1.0",
  "flow_type": "error",
  "message": "Unable to connect to the booking system. Please try again later.",
  "interaction_id": "conv-abc123",
  "status": "backend_unavailable",
  "attribution": { ... }
}
```

---

## 9. Security Considerations

- **Transport:** Intent endpoints **MUST** be served over HTTPS.
- **Attribution:** Implementations **SHOULD** sign chain entries and verify signatures; algorithm and key distribution are implementation-defined.
- **Replay:** Use of a strong `nonce` and `timestamp` **SHOULD** be enforced to limit replay attacks.
- **PII:** Intent and messages may contain personal data; implementers **MUST** comply with applicable privacy regulations (e.g. GDPR) and **SHOULD** minimize retention and exposure.

---

## 10. Protocol Versioning

- `protocol_version` in the envelope and `manifest_version` in the manifest indicate compatibility.
- New minor versions **SHOULD** be backward-compatible (e.g. new optional fields, new flow types that older clients can ignore).
- Incompatible changes **SHOULD** use a new major version; clients and IntentSites may support multiple major versions during transitions.

---

## 11. Summary: Protocol Checklist

| Component | Spec section | Required |
|-----------|--------------|----------|
| Intent Manifest at `{origin}/intentmanifest.yaml` | § 2 | Yes |
| POST to `intent_endpoint` with JSON envelope | § 3 | Yes |
| `flow_type` + `message` + `interaction_id` + `attribution` | § 3, 5 | Yes |
| Markdown allowed in `message` | § 4 | Optional but recommended |
| Attribution chain with signatures | § 5 | Yes |
| Explicability | § 6 | Optional |
| Incremental resolution (backend-driven) | § 7 | Behavioral contract |
| Errors via `flow_type: "error"` | § 8 | Recommended |

---

---

## 12. Related Work and Inspiration

**[NLWeb](https://github.com/nlweb-ai/NLWeb)** provides a natural-language query protocol and MCP server that returns Schema.org JSON. It focuses on **information access** (retrieval, summarization). IntentWeb focuses on **execution** (stateful intent fulfillment, attribution, open discovery). The two are complementary: a site can expose both an NLWeb-style `/ask` for informational queries and an IntentWeb `/intent` for transactional intents. See [IntentWeb and NLWeb](IntentWeb-and-NLWeb.md) for a detailed comparison and ideas adopted from NLWeb (e.g. optional Schema.org in responses, MCP/A2A alignment).

---

*This specification is a draft aligned with [The IntentWeb Architecture Vision](../architecture/The-IntentWeb-Architecture-Vision.md). We invite implementation and feedback to evolve the protocol.*

© 2025 UXHuman Initiative / Smart Goldfish SAS  
Licensed under the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).
