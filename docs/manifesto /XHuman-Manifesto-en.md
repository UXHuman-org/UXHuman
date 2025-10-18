# UXHuman – Open Source Framework for the IntentWeb

© 2025 Mauricio Aristizabal — Canonical repository under the **UXHuman-org** organisation  
Domain: [UXHuman.org](https://uxhuman.org)

---

## Overview

**UXHuman** is an open framework defining the human, ethical, and interactional layer of the emerging **IntentWeb** —  
a distributed web architecture where **intent**, not attention, is the organizing principle.

The framework establishes a unified approach for systems that interpret and act on human intent expressed in natural language.  
It provides the conceptual, technical, and governance foundations that enable companies, institutions, and developers to participate directly in the **intent economy** — without dependence on closed AI intermediaries.

---

## Background

The web’s attention-based model has reached its limit.  
As large AI platforms increasingly mediate user queries and actions, individual organizations risk losing visibility, attribution, and direct access to intent.

UXHuman proposes an open alternative: a standardized, interoperable way for any entity to **receive, interpret, and fulfill intent** through natural language interfaces and machine-executable protocols.

---

## Framework Structure

UXHuman consists of five interrelated documents:

- **[Manifesto](https://github.com/UXHuman-org/UXHuman/blob/main/docs/manifesto/UXHuman-Manifesto-en.md)** — Defines the purpose, scope, and four foundational values: *Language*, *Trust*, *Mental Load*, and *Agency*.  
  These values are expressed through nineteen design principles with measurable criteria.

- **[Economic Model](https://github.com/UXHuman-org/UXHuman/blob/main/docs/economic-model/UXHuman-Economic-Model-en.md)** — Describes how value flows in an intent-based ecosystem.  
  It differentiates between *direct intent fulfillment* and *gateway-mediated orchestration*, establishing the basis for sustainable attribution and revenue alignment.

- **[Architecture Vision](https://github.com/UXHuman-org/UXHuman/blob/main/docs/architecture/The-IntentWeb-Architecture-Vision.md)** — Specifies the core architectural constructs: *Intent Manifests*, *natural language as protocol*, *incremental resolution*, *attribution chains*, *open discovery*, and *adaptive presentation*.  
  Together, they outline the foundational mechanisms of the IntentWeb.

- **[Design Playbook](https://github.com/UXHuman-org/UXHuman/blob/main/docs/design-playbook/UXHuman-Design-Playbook.md)** — Provides implementation patterns for applying the nineteen UXHuman principles in production environments.  
  Includes reference patterns, anti-patterns, and measurable outcomes.

- **[Governance and Collaboration Framework](https://github.com/UXHuman-org/UXHuman/blob/main/docs/governance/UXHuman-Governance-%26-Collaboration-Framework.md)** — Defines contribution, certification, and maintenance processes.  
  Establishes working groups, RFC procedures, and review standards to ensure openness and accountability.

---

## Relationship to the IntentWeb

UXHuman operates as the **human and governance layer** of the IntentWeb.  
Implementations adhering to UXHuman principles — referred to as **IntentSites** — expose capabilities that can interpret and execute intent through natural language.  
When IntentSites interoperate through shared standards, they collectively form the **IntentWeb**, an open network for intent resolution and execution.

---

## Repository Contents

- `/docs/` — Canonical documents defining the UXHuman framework  
- `/proof/` — Cryptographic proofs, version seals, and release attestations  
- `/LICENSE` — Applicable license information (CC BY 4.0 / CC BY-SA 4.0)

Each release is versioned and timestamped for traceability and integrity verification.

---

## Versioning and Provenance

All canonical documents are versioned under semantic tags (`v1.0`, `v1.1`, etc.).  
Each release includes:

- A consolidated archive (`UXHuman-vX.X-docs.zip`)  
- Corresponding SHA-256 checksums and optional OpenTimestamp proofs (`.ots`)  
- Verified records stored in `/proof/<version>/`

This approach ensures verifiable provenance across revisions.

---

## Licensing

- Default license: **Creative Commons Attribution 4.0 International (CC BY 4.0)** — see [LICENSE](https://github.com/UXHuman-org/UXHuman/blob/main/LICENSE)  
- Specific documents: **Creative Commons Attribution–ShareAlike 4.0 (CC BY-SA 4.0)**, where noted  
- Visual assets, diagrams, and trademarks (UXHuman™, IntentWeb™, IntentSite™) remain proprietary © Smart Goldfish SAS

---

## Contribution Model

UXHuman follows an **open stewardship** model.  
Participation is encouraged through documentation reviews, translations, and RFC proposals aligned with the Governance Framework.  
Submissions are evaluated based on clarity, consistency with the Manifesto, and measurable impact on framework evolution.

---

## Stewardship

The UXHuman Initiative is currently stewarded by  
**[Smart Goldfish SAS](https://smartgoldfish.ai)**, based in Sophia Antipolis, France.

Smart Goldfish contributes the initial research, architecture, and reference implementations that support the IntentWeb ecosystem, including IntentSite and IntentLayer.  
Stewardship may transition to a foundation structure as adoption expands.

---

> *UXHuman defines the human interface of the IntentWeb: open, interpretable, and aligned with intent rather than attention.*
