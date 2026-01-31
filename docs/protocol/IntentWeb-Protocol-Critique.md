# IntentWeb Protocol: Critical Analysis

**Purpose:** Honest assessment of weak points, ambiguities, and room for improvement.  
**Audience:** Spec authors and implementers.  
**Tone:** Direct; not intended to dismiss the protocol but to harden it.

---

## 1. Attribution and Cryptography: Dangerously Underspecified

**The protocol requires attribution and signatures but leaves almost everything "implementation-defined."** That is a recipe for non-interoperability and false confidence in "trust."

| Issue | What the spec says | Why it's weak |
|-------|--------------------|----------------|
| **query_hash** | "Hash of the initial intent payload (or canonical representation)" | "Or canonical representation" is a loophole. No canonicalization is defined. Two implementations will hash different byte sequences and get different hashes; verification fails. You need a strict canonical form (e.g. JSON Canonicalization Scheme or a defined field list and encoding). |
| **Signature algorithm** | "Algorithm and encoding (e.g. base64) **SHOULD** be documented or indicated" | **SHOULD** and "documented or indicated" mean the protocol does not mandate any algorithm or encoding. There is no `alg` or `encoding` field. Implementations cannot verify each other's signatures without out-of-band agreement. |
| **Key distribution** | Not mentioned | Who trusts whom? How does an IntentSite get the gateway's or agent's public key? No JWKS, no certificate field, no key URL. Attribution is useless for verification without this. |
| **What is signed** | "e.g. query_hash + nonce + timestamp + actor_id" | "e.g." is not a spec. The exact payload (order, encoding, inclusion of previous chain entry, etc.) must be normative. Otherwise signatures are not reproducible. |

**Recommendation:** Define a normative profile: e.g. Ed25519 or ECDSA P-256, a deterministic serialization of the signed payload, and a mandatory `alg` (and optionally `kid` / key URL) in each chain entry. Publish a separate crypto profile document if needed.

---

## 2. Natural Language as Protocol: No Extraction Contract

The protocol says intent and parameters live in natural language and that "LLMs format, backends provide facts." **But there is no contract for what the IntentSite expects to receive or how it extracts structured data.**

- **No parameter schema:** The manifest has natural-language `requires` (e.g. "Number of people in your party") but no machine-parseable parameter names or types. So the IntentSite cannot reliably say "I have party_size and date, I need time." The `collected_information` and `required_information` are free-form strings; an agent cannot programmatically map "time" to a slot or validate format.
- **Extraction responsibility:** The spec says the "semantic layer (LLM)" extracts and the backend validates. It does not say whether the client may send structured parameters (e.g. in an optional `parameters` object) or only natural language. If only NL, every IntentSite must implement its own NLU; if structured is allowed, the protocol should say so and define a minimal structure.
- **Ambiguity and clarification:** When the IntentSite sends `clarification_request`, there is no standard way to express "which slot is ambiguous" or "here are the N options." It's all in `message`. Fine for humans, brittle for agents that need to present choices or resubmit.

**Recommendation:** Allow an optional `parameters` object (key-value) alongside `message`, and/or define a minimal manifest extension for parameter names and types. Specify that `required_information` can reference these names so agents know what to fill. Define a simple structure for clarification (e.g. `clarification_options: [{ "id", "label" }]`) so agents can render choices consistently.

---

## 3. Manifest: Discovery Only, No Operational Contract

The manifest is good for **discovery** (what intents are offered) but says almost nothing about **how** to use them at runtime.

- **No auth:** The manifest does not indicate whether the intent endpoint requires authentication, or what scheme (API key, OAuth2, etc.). Implementers must guess or out-of-band.
- **No rate limits or quotas:** No hint of rate limits, which matters for gateways and crawlers.
- **Capability versioning:** If a company changes "Book a table" to require a new field, there is no capability-level version or deprecation. `last_updated` is at manifest level only.
- **Stable intent identifiers:** Intents are free-form strings ("Book a table for dining"). For routing and analytics, a stable machine-readable id (e.g. `intent: "book_table"`) would help; the spec could make it optional but recommended.

**Recommendation:** Add optional manifest fields: `auth` (e.g. "none" | "api_key" | "oauth2"), `rate_limit` (e.g. requests per minute), and per-capability `id` and optionally `version` or `deprecated`.

---

## 4. Flow Types and Turn Semantics: Underconstrained

- **Bidirectional flow types:** `information_request` and `clarification_request` are "Either" direction. The spec does not define who sends what when. Can the client send `information_request` ("Do you have vegan options?") in the same turn as an `information_response`? Can the IntentSite send two `information_request` messages in a row for two different slots? Unclear.
- **No turn or sequence number:** There is no `turn_id`, `sequence`, or `previous_message_id`. In multi-turn flows, duplicate or out-of-order delivery cannot be detected. Idempotency is not addressed.
- **Terminal states:** The spec says a new intent requires a new `interaction_id`. It does not say whether the client can **cancel** an intent (e.g. flow_type `cancel` or `abort`) so the IntentSite can release resources and stop sending reminders.

**Recommendation:** Add a normative turn order (e.g. client and server alternate, or define allowed transitions). Add an optional `turn` or `sequence` and recommend idempotency keys for critical operations. Consider a `cancel` or `abort` flow type.

---

## 5. interaction_id: Client-Generated and Unbound

- **Client-generated:** The client chooses `interaction_id`. There is no server-side binding or registration. A malicious or buggy client can reuse the same id across unrelated conversations or across different IntentSites, causing confusion or cross-talk.
- **Uniqueness:** The spec does not require uniqueness across time or space. Best practice (e.g. UUID) is not stated.
- **Session lifetime:** Nothing is said about how long an IntentSite should keep state for an `interaction_id`, or what to do when the client resumes after a long gap.

**Recommendation:** Recommend UUIDv4 or similar and document that reuse of the same `interaction_id` for a different logical conversation is invalid. Optionally allow the server to return a `server_interaction_id` and recommend using it in subsequent requests. Specify or recommend session TTL and behavior on expiry (e.g. 410 Gone or a specific error status).

---

## 6. Discovery: Crawl-Only and Silent on Scale

Discovery is "crawl well-known path" and "follow links." There is no:

- **Registry or index:** So every gateway must crawl every origin it cares about. Fine for dozens of IntentSites; unclear for millions.
- **Freshness:** No `Cache-Control` or `last_updated` semantics for when to re-crawl. Stale manifests can route to outdated endpoints or capabilities.
- **Push or notification:** No way for an IntentSite to notify gateways that its manifest changed. Gateways poll only.

This is acceptable for an early-stage protocol but should be explicitly called out as a scaling limit. Alternative discovery mechanisms (e.g. optional registration, or manifest indexing services) could be documented as future work.

---

## 7. Errors: No Code Taxonomy or Retry Semantics

- **status is free-form:** "e.g. `invalid_request`, `backend_unavailable`, `rate_limited`" is illustrative only. There is no registry of error codes. Implementers and agents cannot rely on a fixed set of values for retry, backoff, or user messaging.
- **HTTP status:** The spec does not map protocol-level `flow_type: "error"` to HTTP status codes. Should 4xx always accompany `error`? Should 503 imply retry? Unspecified.
- **Retry and idempotency:** No guidance on which errors are retriable or how to retry without double-execution (e.g. booking twice).

**Recommendation:** Define a small set of normative error codes (e.g. `invalid_request`, `missing_information`, `backend_unavailable`, `rate_limited`, `session_expired`, `conflict`) and recommend HTTP status mapping. Add a short section on retry (e.g. retry on 5xx and `backend_unavailable` with backoff; do not retry on `invalid_request` without changing the request).

---

## 8. Intent UI: Mentioned but Not Specified

The spec says there is an "Intent UI" at `GET {origin}/intent-ui/` for "human in browser" and that "the same backend logic **SHOULD** drive both API and UI." **Nothing else is specified:** no HTML structure, no client-side protocol (e.g. how the browser sends intent or receives incremental responses), no accessibility or i18n requirements. So "Intent UI" is a placeholder, not part of the protocol.

Either specify a minimal contract (e.g. HTML page that accepts intent via query or form and can receive markdown responses, with optional JS for incremental flow) or rename to "human-facing UI is out of scope for this protocol" and drop the table row.

---

## 9. Security: Gaps Beyond HTTPS

- **PII in message:** The spec correctly says intent and messages may contain PII and that implementers must comply with privacy law. It does not suggest minimization (e.g. avoiding sending full names in logs, or supporting redaction). Guidance would help.
- **Encryption at rest:** Not mentioned. Many deployments will need to store conversations; the spec could recommend encryption at rest and short retention.
- **Signature verification:** "IntentSites **MUST** verify the chain" but the chain cannot be verified without normative crypto. So in practice verification is optional or best-effort.

Tighten the crypto profile first; then "MUST verify" becomes meaningful.

---

## 10. Versioning and Compatibility: Vague

- **Backward compatibility:** "New minor versions **SHOULD** be backward-compatible" and "Incompatible changes **SHOULD** use a new major version" are good principles but not a contract. What does a client that supports "1.0" do when it sees `protocol_version: "1.1"` and new optional fields? What if it sees `"2.0"`? The spec does not define version negotiation or mandatory ignorability of unknown fields.
- **Manifest vs. protocol version:** `manifest_version` and `protocol_version` can diverge. Can a 1.0 client talk to an IntentSite that publishes manifest_version 1.1? Unclear.

**Recommendation:** State that clients MUST ignore unknown top-level fields and unknown flow types (or treat them as generic message). Define that minor version increase = additive only; major = may break. Optionally recommend that clients send supported protocol versions and servers respond with the version they use.

---

## 11. What the Protocol Does Well (Brief)

- **Flow types** give a clear, small vocabulary for interaction state; agents know whether they're initiating, answering, or done.
- **Markdown in message** is a pragmatic choice: one format for humans and LLMs, presentation left to the consumer.
- **Manifest at a well-known path** is simple and consistent with sitemap-style discovery.
- **Required attribution** raises the bar; the problem is making it verifiable, not the idea.
- **Backend-driven requirements** (Concept 4) are correctly encoded as a behavioral contract; the missing piece is a tighter link between manifest/parameters and those requirements.

---

## 12. Summary: Priorities for Improvement

| Priority | Area | Action |
|----------|------|--------|
| **P0** | Attribution / crypto | Define canonical form for query_hash; normative signature algorithm and payload; key distribution (e.g. alg, kid, key URL). |
| **P0** | Errors | Define a small set of error codes and recommend HTTP mapping and retry behavior. |
| **P1** | Parameters / extraction | Allow optional structured parameters; optional manifest extension for parameter names; optional clarification structure. |
| **P1** | Turn and idempotency | Add turn/sequence and idempotency guidance; consider cancel/abort. |
| **P2** | interaction_id | Recommend UUID; document uniqueness and optional server-side id; session TTL. |
| **P2** | Manifest | Optional auth, rate_limit, capability id/version. |
| **P2** | Intent UI | Either specify a minimal contract or state it is out of scope. |
| **P3** | Discovery scale | Document crawl-only as current limit; optional future work (registry, freshness, push). |
| **P3** | Versioning | Explicit unknown-field and version negotiation rules. |

---

*This critique is intended to strengthen the IntentWeb Protocol. Addressing P0 and P1 items would significantly improve implementability and interoperability.*

© 2025 UXHuman Initiative / Smart Goldfish SAS  
Same license as the protocol specification.
