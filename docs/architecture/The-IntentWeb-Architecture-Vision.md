# The IntentWeb Architecture Vision

**A vision for natural language as universal protocol**

---

## 1. How to Read This Document

This document presents the **architectural vision** for the IntentWeb—a proposal for how companies can participate directly in the intent economy through natural language as a universal protocol.

### About This Document

An **architecture vision** that presents core concepts and invites community exploration. This is not a complete specification, implementation guide, or finished standard. It's a starting point for discussion, experimentation, and evolution.

### Document Structure

- **Executive Summary**: Complete overview of problem, solution, values, and concepts
- **Six Core Concepts** (Section 3): Deep dive into each architectural pattern (~2 pages each)
- **How It Fits Together** (Section 4): Complete example showing all concepts in practice
- **Conclusion** (Section 5): Invitation to the community

---

## 2. Executive Summary

### The Problem

The Web was built to connect people with companies. That pact is broken. Intent has become the new currency of the Web, but only AI giants can accept it—shutting companies out while scraping their content and silencing their brands.

### The Solution

This architecture enables companies to accept intent directly through natural language—not just for humans, but as a universal protocol for machines talking to machines. This allows companies to participate in the intent economy rather than being disintermediated by **AI actors** (AI giants like Google and OpenAI, and AI agents acting on behalf of users).

### Four Foundational Values

The architecture is governed by four values from the **UXHuman Manifesto**—not guidelines, but **architectural constraints**:

|UXHuman Value|Architectural Expression|
|---|---|
|**Language**|Natural language is the primary protocol. Structured data supplements intent but never replaces it.|
|**Trust**|Every exchange preserves intent fidelity through cryptographic attribution and explicability.|
|**Mental Load**|Systems minimize cognitive effort through context preservation, clarification, and simplicity.|
|**Sovereignty**|Companies own their execution and knowledge; users own their data and consent.|

### Six Core Concepts

These values translate into six architectural concepts:

**1. Natural Language as Protocol** — Natural language becomes the universal way machines exchange intent, eliminating rigid API schemas.

**2. Markdown as Semantic Structure** — Responses use markdown to provide structured content that any system can interpret and present according to its capabilities.

**3. Adaptive Presentation** — Companies provide semantic content; AI actors decide how to present it. No UI prescription—just meaningful data that adapts to each medium.

**4. Incremental Intent Resolution** — Information requirements come from backend business logic. Systems track progress through multi-turn interactions, gathering information incrementally until sufficient to execute.

**5. Trust Through Attribution & Explicability** — Every intent exchange includes cryptographic proof of who handled it and transparent explanation of why decisions were made.

**6. Open Discovery** — Companies publish capabilities in natural language manifestos that AI actors can discover without permission or gatekeepers.

### What This Enables

**For Users:** Express needs naturally without learning interfaces. Accessible to elderly, disabled, and non-native speakers.

**For Companies:** Recapture traffic lost to AI intermediaries. Own customer relationships. Build one interface serving both humans and AI actors.

**For AI Actors:** Universal protocol to interact with any company. Orchestrate multi-step tasks seamlessly.

**For Developers:** Build once, work everywhere. Evolve capabilities without breaking integrations.

### This Is an Invitation

This is an **architecture vision**, not a complete specification. We invite the community to critique, build, experiment, and evolve these ideas. This vision succeeds if it sparks exploration and helps shape how the web adapts to the age of AI.

**Let's build it together.**

---

## 3. Six Core Concepts

### 3.1 Natural Language as Protocol

_Realizes: Language value_

Natural language becomes the universal way humans and machines exchange intent. Instead of rigid API schemas that require documentation, negotiation, and version management, this architecture uses the language we already speak—making every system immediately interpretable to any actor with language understanding.

#### The Challenge

For decades, machines communicated through structured protocols: REST APIs with JSON schemas, GraphQL with type systems, SOAP with XML definitions. Each integration required:

- Negotiated contracts between parties
- Extensive documentation
- Version management and breaking changes
- Separate interfaces for humans (websites) and machines (APIs)
- Custom integration code for each relationship

This works for stable, high-volume integrations between known partners. But it creates friction for everything else: exploratory queries, one-time requests, new capabilities, and any interaction where the cost of formal integration exceeds the value of the exchange.

**Most critically, it bifurcates the web:** humans get visual interfaces, machines get structured APIs, and the two never converge.

#### The Approach

This architecture proposes natural language as the **primary protocol** for both human-to-machine and machine-to-machine communication.

**A simple example:**

**Traditional API approach:**

```
POST /api/v2/reservations
Content-Type: application/json

{
  "party_size": 2,
  "guest_name": "Jane Smith",
  "date": "2025-10-15",
  "time": "19:00",
  "special_requests": "window seat"
}
```

_Requires: API documentation, authentication setup, schema knowledge, error code handling_

**This architecture's approach:**

```
POST /intent
Content-Type: application/json

{
  "message": "Book a table for 2 people under Jane Smith 
              on October 15 at 7pm. Window seat if possible.",
  "interaction_id": "conv-abc123",
  ...
}
```

_Requires: Natural language capability (which LLMs provide universally)_

The semantic content is in the natural language message. The JSON wrapper handles only routing concerns (interaction ID, attribution, metadata). The company's IntentSite interprets the natural language, extracts parameters, validates against backend requirements, and responds—also in natural language wrapped in JSON.

**This works because:**

- LLMs can parse natural language with high accuracy
- Intent is self-describing ("book a table" clearly indicates the goal)
- Parameters are embedded naturally ("for 2 people" = party_size: 2)
- Context accumulates across interaction turns
- No schema negotiation needed—language itself is the schema

**Both humans and AI actors use the same interface:** `/intent-ui/` for human browser access, `/intent/` for machine access, but both resolve through the same natural language layer.

#### Why This Matters

**Universal compatibility:** Any system with language understanding (every modern LLM) can interact with any IntentSite. No custom integration code required.

**Human-machine parity:** The same interaction works whether a human types it in a browser or an AI agent sends it programmatically. This realizes the **Language value**—natural language becomes the universal interface, eliminating the cognitive load of learning different systems.

**Evolutionary flexibility:** Companies can change backend requirements, add capabilities, or modify constraints without breaking integrations. The natural language interface adapts—just as humans adapt to new menu items or policies without "version updates."

**Self-describing capabilities:** Natural language intent is self-describing. An AI actor encountering "book a table for 2 at 7pm" immediately understands the capability without documentation. REST endpoints like `POST /api/v2/reservations` require external documentation to understand parameters, formats, and constraints.

#### Comparison with Current Approaches

The industry is experimenting with multiple paths to make the Web machine-readable and agent-accessible. Each approach tackles the same core challenge — bridging human intent and system execution — but from different angles.

---

**1. Browser Automation (Current Stopgap)**  
Today, some AI systems attempt machine interaction through **browser automation** — programmatically clicking buttons and filling forms on human interfaces. While creative, this approach faces fundamental limitations:

- **Token inefficiency:** Processing entire HTML pages (often 500 KB+) to extract simple data wastes thousands of tokens per interaction.  
- **High fragility:** A minor layout or DOM change can break the flow.  
- **Latency:** Rendering and parsing add seconds to every interaction.  
- **Opacity:** When automation fails, AI must guess what happened (“Did the booking succeed?”).  

Browser automation is a temporary bridge. It mimics human behavior instead of enabling true protocol-level communication.

---

**2. NLWeb and the Move Toward a Language-Aware Web**  
A more structured evolution is represented by **NLWeb**, led by *R.V. Guha* at Microsoft Research. NLWeb enables websites to expose natural-language query interfaces for humans and to provide **structured agent access** through the Model Context Protocol (MCP).  
This allows content to become both **conversational and machine-discoverable**, reducing friction between traditional web data and AI agents. NLWeb’s focus, however, remains primarily on **information access** — enabling retrieval, summarization, and semantic navigation of existing content through natural language.

---

**3. IntentWeb: From Access to Execution**  
IntentWeb extends the same frontier but moves from *understanding* content to *acting* on it. It defines how intent is **expressed, verified, and fulfilled** across organizations and agents — incorporating trust, attribution, and governance as architectural primitives.  
Where NLWeb connects language to information, **IntentWeb connects language to action**. It transforms natural language from a descriptive layer into a transactional one, enabling verifiable execution rather than passive retrieval.

---

**In summary:**  
- **Browser automation** imitates interaction.  
- **NLWeb** formalizes access.  
- **IntentWeb** operationalizes execution.  

Together, these approaches trace the Web’s evolution — from pages, to language, to intent.

---

### 3.2 Markdown as Semantic Structure

_Realizes: Language value_

Responses use markdown to provide structured, semantic content that any system can interpret and present according to its capabilities. Markdown is not about prescribing UI—it's about communicating meaning and relationships in a format that both humans and LLMs can understand.

#### The Challenge

When machines exchange structured information, they traditionally use formats designed for machine parsing: JSON for data, XML for documents, HTML for presentation. Each serves a purpose but creates limitations:

**JSON** is structured but semantic-poor. `{"price": 79, "name": "Headphones"}` carries data but not relationships, emphasis, or context.

**XML** adds hierarchy but remains verbose and machine-centric. Humans struggle to read it.

**HTML** mixes content with presentation. `<div class="product-card">` tells you nothing about what the data means—only how to display it.

None of these formats are **universally interpretable**—they require either human effort (reading HTML source) or schema knowledge (understanding JSON structure). And none work well when the same content must be presented differently across voice, text, mobile, desktop, and future mediums we haven't invented yet.

#### The Approach

This architecture uses **markdown as the semantic interchange format**—a lightweight way to communicate structure, relationships, and emphasis that both humans and LLMs can interpret.

**Example: Product search response**

``` 
# Wireless Headphones Under $100

| Product               | Price | Battery | Rating |
|-----------------------|-------|---------|--------|
| SoundCore Q30         | $79   | 40h     | 4.5/5  |
| Anker Soundcore Life  | $49   | 40h     | 4.4/5  |
| JBL Tune 510BT        | $59   | 60h     | 4.3/5  |

**Best value:** JBL Tune 510BT offers longest battery life at competitive price.

[View all products](https://company.com/headphones)
```

**This markdown carries semantic meaning:**

- **Table structure** = comparable data (products with attributes)
- **Emphasis** (bold "Best value") = highlighted recommendation
- **Link** = actionable destination
- **Heading** = topic organization

**Different consumers interpret this differently:**

**Voice assistant:** "I found three wireless headphones under $100. The SoundCore Q30 costs $79 with 40 hours of battery..."

**Mobile app:** Renders product cards with images, tap to compare, buy buttons

**Text terminal:** Displays ASCII table with aligned columns

**Advanced AI agent:** Extracts: "JBL has best battery-per-dollar ratio" → Recommends JBL

**Same semantic content. Different interpretations. Different presentations.**

The company provides **what the data means**. The consumer decides **how to present it**.

#### Why This Matters

**Semantic flexibility:** Consumers extract meaning and transform content for their medium. Voice narrates, visuals display, agents analyze—all from the same source.

**LLM-interpretable:** Modern LLMs excel at understanding markdown structure. They can parse tables, extract relationships, understand emphasis, and reason about content without custom parsing logic.

**Future-proof:** New presentation mediums (AR glasses, brain-computer interfaces, whatever comes next) can interpret the same markdown without companies rewriting responses.

**No UI prescription:** Companies don't dictate "show as grid" or "use carousel." They provide structured meaning; consumers respect their users' context and capabilities.

This realizes the **Language value**—markdown extends natural language with just enough structure to communicate relationships while remaining universally interpretable.


---

### 3.3 Adaptive Presentation

_Realizes: Sovereignty value_

Companies provide semantic content in markdown; AI actors and applications adapt this content for their presentation medium—voice, visual interface, text terminal, or intelligent analysis. This separation preserves company sovereignty over what is communicated while respecting consumer sovereignty over how it's presented.

#### The Challenge

Traditional web architecture forces companies to make presentation decisions for users they'll never meet, on devices they can't predict, in contexts they can't anticipate. Should product listings be grids or lists? Should navigation be hamburger menus or tab bars? How should content adapt for screen readers, small screens, or voice interfaces?

**The usual solution:** Build multiple versions. Desktop website. Mobile website. Mobile app (iOS and Android). Voice skills. Each requires separate development, separate maintenance, separate design decisions. And each still makes assumptions about how users want to interact.

**Worse:** APIs force this presentation logic into the backend. REST endpoints return data structures that assume certain UI patterns—arrays of objects that "should" become grids, hierarchies that "should" become trees. The backend prescribes not just data but its expected presentation.

**This violates sovereignty:** Companies spend resources building UIs for contexts they don't understand. Users receive presentations that don't match their needs or preferences.

#### The Approach

This pattern separates what companies provide (semantic content) from how it's presented (adaptive rendering by the consumer).

**Example: Restaurant availability response**

**Company provides (markdown):**

```
## Available Times on October 15, 2025

| Time  | Table Size    | Location    |
|-------|---------------|-------------|
| 18:00 | 2-4 people    | Main dining |
| 18:30 | 2-4 people    | Main dining |
| 19:00 | 2 people only | Bar area    |
| 19:30 | 2-4 people    | Main dining |
| 20:00 | 2-4 people    | Patio       |
| 20:30 | 2-6 people    | Private room|

**Peak times:** 19:00-20:00 book quickly.
```

**Different consumers adapt differently:**

**Voice assistant (for elderly user in car):** "I have six available times. The earliest is 6pm for 2 to 4 people in the main dining room. Would you like that, or shall I continue?"

**Mobile app (for busy professional):** Visual time picker with color coding (green = available, yellow = limited, red = unavailable). One tap to select.

**Text terminal (for developer testing):**

```
1. 18:00 - Main dining (2-4)
2. 18:30 - Main dining (2-4)
3. 19:00 - Bar area (2 only)
...
Select option:
```

**AI agent (analyzing for user):** "The 20:00 patio seating best matches your preference for outdoor dining and your party size of 4."

**Same information. Each presentation optimized for its context.**

The company provided:

- Available times (data)
- Table constraints (requirements)
- Location options (choices)
- Peak time warning (context)

Each consumer decided:

- How to display options (list, picker, analysis)
- How much detail to show (all at once, one by one)
- What to emphasize (earliest, preferred, all equal)
- Whether to filter or present everything

#### Why This Matters

**Company sovereignty:** Companies control what information is shared, what constraints apply, what recommendations are made—without building UI for every possible presentation.

**Consumer sovereignty:** AI actors and applications know their users' context (driving, browsing, in a hurry, accessibility needs) and adapt presentation accordingly.

**Progressive enhancement:** Basic consumers render simple text or tables. Advanced consumers add visual richness, voice optimization, or intelligent filtering. The same content serves all capability levels.

**Accessibility by default:** Voice interfaces, screen readers, and assistive technologies can adapt markdown to their users' needs without companies building separate "accessible versions."

This realizes the **Sovereignty value**—companies own their content and execution; consumers own the presentation and user experience.

---

### 3.4 Incremental Intent Resolution

_Realizes: Sovereignty + Trust values_

Information requirements come from backend business logic, not AI guessing. Systems track progress through multi-turn interactions, gathering information incrementally until sufficient to execute. This creates deterministic, transparent flows that preserve company sovereignty over execution requirements while building trust through verifiable progress.

#### The Challenge

Traditional web forms demand all information upfront: fill every field, then submit. APIs work the same way—send a complete, valid request or receive an error.

But natural conversation works incrementally: "I want to book a table." → "For how many?" → "Two people." → "When?" → "Tomorrow at 7pm."

**With AI, a critical question emerges:** Who decides what information to request? If the AI decides, it might ask for irrelevant details, skip critical requirements, or invent constraints that don't exist. This violates company sovereignty (AI shouldn't determine business requirements) and creates reliability concerns (AI might guess what's needed rather than know).

#### The Approach

Intent resolves incrementally through a **three-layer architecture** where backend requirements drive interaction flow:

**Backend Systems** → Define requirements (database schemas, business rules, constraints)  
**Intent Resolution Engine** → Track progress (what's collected, what's missing, what's next)  
**Semantic Layer (LLM)** → Format requests naturally (turn "need: party_size" into "How many people?")

**This architecture ensures:**

- Requirements come from authoritative business logic (Sovereignty)
- Progress is tracked deterministically (Trust)
- Interaction flows naturally (Language)

**Intent flow types make interaction state explicit, enabling AI actors to understand progress without parsing content.**

#### Example: Restaurant Booking Ineraction

**Turn 1 - User initiates:**

```json
{
  "flow_type": "intent_request",
  "message": "Book a table for 2 under Jane Smith",
  "interaction_id": "conv-abc123",
  ...
}
```

**Engine state:**

```
Required: [party_size, guest_name, date, time]
Collected: [party_size: 2, guest_name: "Jane Smith"]
Missing: [date, time]
Next action: Request date
```

**System response:**

```json
{
  "flow_type": "information_request",
  "message": "What date would you like to book for?",
  "interaction_id": "conv-abc123",
  "required_information": ["date"],
  ...
}
```

---

**Turn 2 - User provides date:**

```json
{
  "flow_type": "information_response",
  "message": "Tomorrow",
  "interaction_id": "conv-abc123",
  ...
}
```

**Engine state:**

```
Required: [party_size, guest_name, date, time]
Collected: [party_size: 2, guest_name: "Jane Smith", date: "2025-10-16"]
Missing: [time]
Next action: Request time with available options
```

**System queries backend for available times, then responds:**

```json
{
  "flow_type": "information_request",
  "message": "I have these times available on October 16:

| Time  | Availability |
|-------|--------------|
| 18:00 | Available    |
| 18:30 | Available    |
| 19:00 | Available    |
| 19:30 | Limited      |

Which time works for you?",
  "interaction_id": "conv-abc123",
  "required_information": ["time"],
  ...
}
```

---

**Turn 3 - User selects time:**

```json
{
  "flow_type": "information_response",
  "message": "7pm",
  "interaction_id": "conv-abc123",
  ...
}
```

**Engine state:**

```
Required: [party_size, guest_name, date, time]
Collected: [party_size: 2, guest_name: "Jane Smith", date: "2025-10-16", time: "19:00"]
Missing: []
Next action: Validate constraints, execute booking
```

**Backend validation:** ✓ Party size within limits, ✓ Date within booking window, ✓ Time available

**System executes booking, responds:**

```json
{
  "flow_type": "execution_result",
  "message": "# Booking Confirmed

**Booking ID:** RES-2025-10-16-7891

- Party size: 2 people
- Name: Jane Smith
- Date: October 16, 2025
- Time: 19:00

Please arrive 5 minutes early.",
  "interaction_id": "conv-abc123",
  "booking_id": "RES-2025-10-16-7891",
  "status": "confirmed",
  ...
}
```

---

#### Understanding Intent Flow Types

**The flow types signal interaction state explicitly:**

|Flow Type|Purpose|Examples|
|---|---|---|
|`intent_request`|Initiate a new intent|"Book a table for 2 under Smith"|
|`information_request`|Request additional information (bidirectional)|"What date would you like?" / "Do you have vegan options?"|
|`information_response`|Provide requested information (bidirectional)|"Tomorrow at 7pm" / "Yes, we accommodate all dietary restrictions"|
|`clarification_request`|Request disambiguation (bidirectional)|"Did you mean Chicago or Chicago Heights?" / "I meant 3 people, not 2"|
|`execution_result`|Intent successfully completed|"Booking confirmed: RES-12345"|
|`error`|Unrecoverable error|"Unable to connect to booking system"|

**Machine-readable state:** AI actors know if they're initiating, responding, or receiving results without parsing natural language content.

**Progress tracking:** The flow type sequence (`intent_request` → `information_request` → `information_response` → `execution_result`) shows deterministic progression.

**Error handling:** Flow types like `error` or `clarification_request` make failure modes explicit.

**Orchestration:** AI agents can chain intents across multiple companies by recognizing when one intent completes (`execution_result`) and another can begin.

#### How This Addresses Reliability Concerns

This architecture substantially reduces hallucination risk through its layered design:

**LLMs format, backends provide facts.** When the system responds "I have these times available on October 16: [table]", every fact comes from the booking database. The LLM didn't generate "18:00 is available"—it formatted the database result as markdown. The available times, the table data, the constraints—all from authoritative systems. The LLM orchestrates (calls `get_available_times(date="2025-10-16")`) and formats (creates the markdown table), but never invents factual information.

**Parameters are extracted, not generated.** When the user says "tomorrow at 7pm", the LLM extracts structured data: `date="2025-10-16"`, `time="19:00"`. Modern LLMs excel at this extraction task with high accuracy using techniques like structured outputs and JSON mode. If extraction fails or produces ambiguous results, the system issues a `clarification_request`—it doesn't guess.

**Backend validates everything.** Even if the LLM somehow extracts `party_size: 100`, the backend rejects it against business rules ("parties larger than 20 require events team"). Invalid dates, unavailable times, constraint violations—all caught by authoritative systems before execution. The backend is the final arbiter of what's valid.

**Flow types prevent invented state.** The explicit flow types mean the LLM can't pretend an intent is complete when it's not. The system tracks required vs. collected information deterministically. The LLM can't output `execution_result` until the backend confirms all requirements are met and execution succeeded.

**Continuous improvement.** As LLMs improve at structured extraction and function calling, the architecture benefits immediately. Each model generation brings better parameter accuracy, more reliable orchestration, and fewer extraction errors. The architectural pattern remains constant while the implementation improves.

**Residual risks remain manageable.** This doesn't eliminate all risks—parameter extraction can have errors (parsing "tomorrow" incorrectly), and formatting mistakes can occur (transposing digits in a table). But the architecture confines the LLM to tasks it handles well (orchestration, natural language formatting) while keeping authoritative data in backend systems where it belongs. For high-stakes operations, deterministic template-based formatting can replace LLM formatting entirely.

#### Why This Matters

**Sovereignty:** Business logic (backend) determines requirements, not AI systems. Companies control what information is necessary, what constraints apply, and when execution can occur. The three-layer architecture preserves this control at every step.

**Trust through determinism:** The interaction flow is predictable and explainable. Flow types make progress transparent and verifiable. Each step follows from backend requirements, not AI invention. Users and companies can trace exactly how the interaction progressed.

**Reliability through architectural separation:** By separating concerns—backends provide facts, engines track state, LLMs format language—the architecture makes each component responsible only for what it does well. This separation is the key to reliable AI-mediated transactions.

**Machine efficiency:** AI actors can process incremental exchanges rapidly. Flow types enable them to understand interaction state without parsing content. Dense information (like tables with 20 time slots) is instantly parseable, enabling efficient multi-turn protocols that would overwhelm human users.


---

### 3.5 Trust Through Attribution & Explicability

_Realizes: Trust + Sovereignty values_

Trust is built through two complementary mechanisms: **attribution** (cryptographic proof of who handled the intent) and **explicability** (transparent explanation of why decisions were made). Together, they create complete transparency—users can verify the chain of custody and understand the reasoning at each step.

#### The Challenge

Intent passes through multiple actors before execution: a user expresses intent to an AI agent, which routes through an AI gateway, which selects an IntentSite to fulfill it. At each hop, the intent could be:

- Modified or misinterpreted
- Routed incorrectly
- Attributed to the wrong source
- Lost or replayed maliciously

Without verifiable trust mechanisms, companies can't verify who sent the traffic, users can't confirm their intent wasn't altered, bad actors can't be identified and blocked, attribution for analytics and pricing becomes impossible, and regulatory compliance with audit trail requirements becomes problematic.

**Traditional solution:** Centralized trust (everyone trusts the platform). But this creates single points of failure and gives platforms unilateral power.

#### The Approach

Trust builds through **decentralized cryptographic verification** and **optional transparent reasoning**.

**Attribution: Verifiable Chain of Custody**

Every message includes an attribution chain showing each actor who handled the intent:

```json
"attribution": {
  "query_hash": "a1b2c3d4...",
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
      "actor_id": "google-gemini-gateway",
      "timestamp": "2025-10-15T19:23:42Z",
      "signature": "sig_gateway_def..."
    }
  ]
}
```

**What this provides:**

**Intent fidelity:** The query_hash proves the intent hasn't been tampered with. Any modification changes the hash.

**Replay protection:** The nonce ensures each request is unique. The same intent can't be replayed maliciously.

**Freshness:** The timestamp proves recency. Stale requests can be rejected.

**Chain of custody:** Each actor's signature proves they handled this specific intent at this specific time.

**Non-repudiation:** Actors cannot deny their involvement. The cryptographic signature is proof.

**Verification:** The IntentSite can verify every signature in the chain, confirming that each actor is who they claim to be and that the intent path is legitimate.

**Explicability: Transparent Reasoning (Optional)**

Beyond proving WHO handled the intent, actors can explain WHY they made their decisions:

```json
"explicability": [
  {
    "actor": "ai_agent",
    "interpretation": "User wants wireless headphones for running",
    "assumptions": ["wireless preferred", "sweat resistance important"],
    "confidence": 0.85
  },
  {
    "actor": "ai_gateway",
    "routing_logic": "Selected electronics retailers with sports audio inventory",
    "sites_considered": ["company-a.com", "company-b.com"],
    "selection_criteria": "Inventory match + user location + reputation"
  }
]
```

**This optional transparency layer enables:**

**"Why these results?"** Users can understand how their intent was interpreted and why certain companies were selected.

**Debugging:** Developers can trace exactly where misinterpretation occurred.

**Improvement:** AI systems can learn from explicability data to refine routing and interpretation.

**Regulatory compliance:** Transparent decision-making satisfies "explainable AI" requirements in regulated industries.

**Explicability is optional but recommended.** Attribution is required (protocol-level trust); explicability is encouraged (user-facing trust).

#### Why This Matters

**Trust through verification (Attribution):** Companies know exactly who sent traffic. Users know their intent wasn't tampered with. Everyone can verify the cryptographic proof—no central authority needed.

**Trust through understanding (Explicability):** Users understand why they got these results. Companies understand how they were selected. Transparency builds confidence.

**Sovereignty restored:** Companies gain complete attribution trail for analytics, pricing, and abuse detection. They know traffic sources without depending on platforms to self-report.

**Regulatory alignment:** Cryptographic audit trails satisfy compliance requirements (GDPR, financial regulations, healthcare). Explicability satisfies "right to explanation" mandates.

**Accountability:** Bad actors leave cryptographic evidence. Abuse patterns become traceable. Reputation systems can be built on verified behavior.


---

### 3.6 Open Discovery

_Realizes: Sovereignty value_

Companies publish their capabilities in structured manifests—using YAML format with natural language intent descriptions—that AI actors can discover, understand, and use without permission from gatekeepers. This enables permissionless participation in the intent economy.

#### The Challenge

For AI actors to interact with companies, they must first discover:

- What companies exist
- What intents they can handle
- What information they need
- What constraints apply

**Traditional solution:** Central registries—like app stores or API directories. But these create gatekeepers who decide who can participate, what terms apply, and which companies get visibility.

**This violates sovereignty.** Companies must seek permission, accept platform terms, and cede control over their participation.

**Alternative approach:** AI systems scrape websites and guess capabilities. But this is unreliable, inefficient, and violates company intent about what should be machine-accessible.

#### The Approach

Companies publish **Intent Manifests**—structured YAML documents with natural language intent descriptions—at a well-known location: `company.com/intentmanifest.yaml`

**Example: Restaurant Intent Manifest**

```yaml
manifest_version: "1.0"
company: "Bella Cucina Restaurant"
last_updated: "2025-10-15"
schema_type: "https://schema.org/Restaurant"

about: "Traditional Italian restaurant in downtown Chicago. We specialize in fresh pasta, wood-fired pizza, and seasonal ingredients from local farms."

location:
  address: "456 Michigan Avenue, Chicago, IL 60611"
  neighborhood: "River North"
  service_type: "dine-in"

capabilities:
  - intent: "Book a table for dining"
    description: "Reserve a table at our restaurant for lunch or dinner"
    examples:
      - "Book a table for 2 people tomorrow at 7pm"
      - "Reserve dinner for 4 under the name Smith"
      - "Get me a table on Saturday night"
      - "I need a reservation for 6 on the patio"
    requires:
      - "Number of people in your party (we accommodate 1-20)"
      - "Guest name for the reservation"
      - "Preferred date"
      - "Preferred time"
    constraints:
      - "Reservations accepted from 2 hours to 90 days in advance"
      - "Parties larger than 20: please contact our events team"
      - "Peak dinner hours (7pm-9pm) book quickly on weekends"
    operating_hours:
      lunch: "Tuesday through Sunday, 12:00-15:00"
      dinner: "Tuesday through Sunday, 18:00-23:00"
      closed: "Mondays"
  
  - intent: "Cancel a reservation"
    description: "Cancel an existing table booking"
    examples:
      - "Cancel my reservation for tomorrow"
      - "I need to cancel booking RES-12345"
      - "Cancel the reservation under Smith for Saturday"
    requires:
      - "Your booking reference number OR your name, date, and time"
    constraints:
      - "Cancellations must be made at least 2 hours before reservation time"
      - "Late cancellations may incur a fee per our policy"

  - intent: "Modify a reservation"
    description: "Change details of an existing booking"
    examples:
      - "Change my reservation to 8pm instead of 7pm"
      - "Increase party size to 6 people for booking RES-12345"
      - "Move my Saturday reservation to Sunday"
    requires:
      - "Your booking reference number"
      - "What you'd like to change (time, date, or party size)"
    constraints:
      - "Modifications accepted up to 2 hours before reservation"
      - "Subject to availability for new time/date"

  - intent: "Check dietary accommodations"
    description: "Ask about dietary restrictions and special menu options"
    examples:
      - "Do you have gluten-free pasta?"
      - "Can you accommodate a vegan diet?"
      - "I have a severe nut allergy, can you help?"
    requires:
      - "Description of dietary needs or restrictions"
    notes:
      - "We accommodate most dietary restrictions with advance notice"
      - "Please inform us when booking for best preparation"

contact:
  intent_endpoint: "https://bellacucina.com/intent"
  website: "https://bellacucina.com"
  phone: "+1-312-555-BELLA"
  email: "reservations@bellacucina.com"
  events_email: "events@bellacucina.com"
```

**Key features of this format:**

**Structured container (YAML):** Machine-parseable, version-controlled, standard tooling works

**Natural language content:**

- Intent descriptions: "Book a table for dining" (not `book_table`)
- Requirements: "Number of people in your party" (not `party_size: integer`)
- Examples show natural expression
- Constraints explained in plain language

**Optional Schema.org reference:** Provides business type context for AI actors

**Human-readable and machine-indexable:** Both humans and AI systems can understand it

#### How AI Actors Use Manifests

**Crawl:** AI gateways periodically discover manifests at `company.com/intentmanifest.yaml`—just like search engines crawl sitemap.xml

**Index:** Parse YAML structure and understand natural language intents:

- Bella Cucina → handles "book a table", "cancel reservation", "modify reservation"
- Chicago restaurants → [Bella Cucina, other restaurants...]
- Dining reservations → [all restaurant IntentSites]

**Match:** When user expresses intent ("book a table in Chicago tomorrow"), the AI gateway:

- Matches natural language to indexed capabilities
- Identifies companies that can handle this intent
- Considers location, constraints, availability

**Route:** Send the intent to the appropriate IntentSite endpoint

**This is exactly how web discovery works today**—periodic crawling, structured formats, semantic understanding—but for capabilities instead of pages.

#### Why This Matters

**Permissionless participation:** Any company can publish a manifest and participate in the intent economy. No approval needed, no gatekeepers, no platform fees—just like publishing a website with a sitemap.

**Company sovereignty:** Each company describes capabilities in their own words, sets their own constraints, and controls their own terms. No central authority dictates how to express capabilities or what to offer.

**Natural language semantics:** Intents and requirements are expressed as humans would describe them, making manifests both human-readable and LLM-interpretable without rigid schemas.

**Efficient discovery:** YAML structure enables fast parsing and indexing while natural language enables deep semantic understanding. Best of both worlds.

**Standard web pattern:** Uses established web discovery mechanisms—well-known URIs, periodic crawling, structured metadata—making adoption straightforward for companies already publishing sitemaps and robots.txt.

**IntentKeepers become IntentGateways:** As more companies publish manifests, AI platforms gain the capability indexes needed to route intent to execution—transforming from answer engines that scrape content into orchestration platforms that connect intent to action.


---

## 4. How It Fits Together

The six core concepts aren't independent features—they're interdependent architectural patterns that work together to create a feasible, beneficial protocol for the intent economy.

### The Complete Picture

**Discovery happens first.** Companies publish Intent Manifests (Concept 6: Open Discovery) in structured YAML with natural language intent descriptions. AI gateways crawl and index these manifests, building capability maps: which companies handle which intents, with what constraints, in what locations. This permissionless discovery means any company can participate without approval—sovereignty from the start.

**Natural language enables universal access.** When a user expresses intent—"book a table in Chicago tomorrow"—the AI gateway uses its capability index to route the request to matching IntentSites. The request itself is natural language wrapped in JSON (Concept 1: Natural Language as Protocol). No API documentation needed, no schema negotiation, no integration code. Any system with language understanding can participate immediately.

**Incremental resolution gathers requirements.** The IntentSite receives the intent but recognizes missing information. The backend defines requirements: party size, guest name, date, time (Concept 4: Incremental Intent Resolution). The Intent Resolution Engine tracks what's collected versus what's needed. The interaction proceeds incrementally—"How many people?" "What name?" "Which time?"—with the backend remaining authoritative at each step. This architecture naturally grounds the LLM, preventing hallucination by ensuring it only formats verified data, never generates facts.

**Semantic structure enables interpretation.** Responses use markdown to communicate meaning (Concept 2: Markdown as Semantic Structure). When the IntentSite returns available times in a markdown table, it's providing structured semantic content—not prescribing UI. The table structure says "these are comparable options"; the emphasis says "this one is recommended"; the constraints say "here are the limits."

**Adaptive presentation respects context.** The AI gateway or agent receiving the markdown interprets it according to their user's context (Concept 3: Adaptive Presentation). A voice assistant narrates the options one by one. A mobile app renders an interactive time picker. A text interface displays an ASCII table. An intelligent agent analyzes the data and recommends the best match. The same semantic content serves all these needs because presentation is separated from meaning.

**Trust flows through the entire chain.** Every message carries cryptographic attribution (Concept 5: Trust Through Attribution & Explicability). The user's intent is hashed; each actor who handles it adds their signature; the IntentSite verifies the complete chain. This proves who handled the request and that it wasn't tampered with. Optional explicability shows why decisions were made—why this company was selected, why these results were returned. Trust isn't about believing platforms; it's about verifying cryptographic proof.

### Why This Works

Each concept solves a specific problem, but together they create emergent properties:

**Feasibility emerges from familiarity.** Open Discovery uses web crawling patterns companies already understand. Natural Language as Protocol leverages LLM capabilities now universally available. Incremental Intent Resolution formalizes function calling and tool use that production systems already implement. The architecture doesn't require inventing new technology—it standardizes and connects existing patterns.

**Sovereignty emerges from separation.** Companies control their Intent Manifests, their backend requirements, their data, their execution (Concepts 4, 6). AI actors control routing, presentation, and user experience (Concepts 1, 3). Neither dictates the other's domain. This separation makes the protocol viable—no single actor has unilateral power.

**Trust emerges from verification.** Cryptographic attribution (Concept 5) removes the need to trust platforms. Backend grounding (Concept 4) removes the need to trust LLMs with facts. Every claim is verifiable. Every data point has provenance. Trust becomes mathematical, not political.

**Scalability emerges from permissionlessness.** No central registry approves participants. No platform negotiates each integration. Companies publish manifests; AI actors discover them; natural language enables immediate interaction. The protocol scales because it requires no coordination—just as the web scaled because anyone could publish without permission.

**Value emerges from directness.** Intent flows from user through AI actors directly to companies who can fulfill it. No intermediary scrapes and summarizes. No platform captures the relationship. Companies reclaim customer engagement. Users get accurate, grounded responses. AI actors gain execution capability. Everyone benefits because the architecture aligns incentives rather than creating zero-sum competition.

### A Complete Example

**Discovery phase:** Bella Cucina publishes `bellacucina.com/intentmanifest.yaml` describing their booking capability. Google's AI gateway crawls it and indexes: "Bella Cucina in Chicago handles table reservations."

**Intent expression:** User tells their AI agent: "Book me dinner in Chicago tomorrow night." The agent sends this natural language intent to Google's gateway.

**Routing:** The gateway matches "dinner" + "Chicago" to Bella Cucina and routes the request with full attribution chain.

**Incremental resolution:** Bella Cucina's IntentSite recognizes the intent but needs more information. Backend requirements: party size, name, date, time. Collected: date (tomorrow). Missing: party size, name, time. The system requests: "How many people?" User responds: "Two, under Smith." System queries backend for available times and returns a markdown table of options. User selects: "7pm." Backend validates and executes booking.

**Semantic response:** IntentSite returns booking confirmation in markdown—heading, booking ID, details table, instructions. Backend data formatted by LLM, not invented.

**Adaptive presentation:** User's AI agent receives markdown. Since user is driving, it reads aloud: "Your table is confirmed for tomorrow at 7pm, booking reference RES-12345." If user had been browsing on mobile, agent would have shown visual confirmation card.

**Trust verification:** The response includes attribution showing it came from Bella Cucina's booking system at specific timestamp, signed by the IntentSite, gateway, and agent. User can verify the chain. Company can verify traffic source. Everyone has cryptographic proof.

**From discovery to execution, the six concepts work together seamlessly.**

### The Protocol in Practice

This isn't theoretical. Every component uses existing technology:

- YAML manifests (like sitemap.xml)
- Web crawling (like Google's index)
- Natural language APIs (every LLM provider offers these)
- Function calling and tool use (standard LLM features)
- Markdown (universal format)
- Cryptographic signatures (standard practice)

What's new isn't the technology—it's the **architectural pattern** that connects them into a coherent protocol.

This architecture works because it doesn't fight against existing incentives or capabilities. It channels them. Companies want customer relationships—this architecture enables direct engagement. AI actors want execution capability—this architecture provides structured access. Users want natural interaction—this architecture delivers it. Developers want simplicity—this architecture offers one interface, natural language, for both humans and machines.

The six concepts complement each other to create something none could achieve alone: a protocol that's feasible to implement, sovereign in operation, trustworthy by design, and beneficial for every participant.

**This is how these architectural patterns catalyze the web's evolution from an attention economy to an execution economy—not through disruption, but through architectural alignment.**

---

## 5. Conclusion

The web was built to connect people with companies. This architecture vision shows how that connection can be restored—through natural language as a universal protocol that preserves the four foundational values of Language, Trust, Mental Load reduction, and Sovereignty.

The six core concepts present a feasible path forward, built on existing technology and familiar patterns. What remains is implementation, experimentation, and community refinement.

We invite developers, companies, and platform builders to critique these ideas, build reference implementations, and evolve the architecture through real-world testing. This vision succeeds through collective participation, not centralized control.

---

---

## References / Related Work

- **Microsoft** — *Introducing NLWeb: Bringing Conversational Interfaces Directly to the Web* (2025).  
  Official Microsoft announcement introducing **NLWeb**, a framework enabling websites to expose natural-language query interfaces and optional structured access for AI agents through the Model Context Protocol (MCP).  
  [https://news.microsoft.com/source/features/company-news/introducing-nlweb-bringing-conversational-interfaces-directly-to-the-web/](https://news.microsoft.com/source/features/company-news/introducing-nlweb-bringing-conversational-interfaces-directly-to-the-web/)

- **Anthropic** — *Introducing the Model Context Protocol (MCP)* (Nov 25, 2024).  
  Official Anthropic publication defining **MCP** as an open protocol for connecting AI systems to external tools, data sources, and contextual resources in a structured and secure way.  
  [https://www.anthropic.com/news/model-context-protocol](https://www.anthropic.com/news/model-context-protocol)

- **Google** — *A2A: A New Era of Agent Interoperability* (2025).  
  Official Google Developers announcement introducing the **Agent-to-Agent (A2A)** protocol, designed to enable secure collaboration and interoperability between AI agents across platforms.  
  [https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)

---

*Together, these initiatives illustrate the Web’s shift from static content and closed APIs toward open, language-aware, and agent-accessible architectures — the foundation upon which IntentWeb builds its execution, trust, and governance layers.*



© 2025 UXHuman Initiative / Smart Goldfish SAS
Licensed under the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).  
If you remix or build upon this material, you must distribute your contribution under the same license,  
with attribution to “UXHuman" (www.uxhuman.org).  
See the full license at https://creativecommons.org/licenses/by-sa/4.0/

Visual assets (logos, icons, and illustrations) are excluded from this license and remain the property of Smart Goldfish SAS.  
UXHuman™, IntentWeb™, and IntentSite™ are registered or claimed trademarks of Smart Goldfish SAS.  
Unauthorized commercial or misleading use of these marks is prohibited.
