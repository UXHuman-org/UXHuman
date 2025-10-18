
# UXHuman Governance & Collaboration Framework

**How the ecosystem evolves — fairly, transparently, and together**

---

## **1. Purpose & Scope**

This document defines how **UXHuman**—the framework that enables the IntentWeb—is maintained, evolved, and protected through open, transparent, and sovereign governance.

It establishes the structures, rules, and processes that ensure 
the ecosystem:

- Remains open and non-proprietary
    
- Evolves through meritocratic contribution
    
- Aligns with the four immutable values — **Language, Trust, Mental Load, and Sovereignty**
    
- Operates sustainably, transparently, and free from capture
    
This Framework is the **highest procedural authority** within the 
UXHuman canon.

All future annexes, RFCs, or certifications operate under its scope 
and cannot contradict these four values.

---

## 2. Core Governance Principles

| Principle          | Description                                                                                                               |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Sovereignty**    | Every organization controls its own IntentSite, data, and execution. Governance ensures interoperability, not dependency. |
| **Transparency**   | All decisions, votes, and financial reports are public and traceable via open repositories.                               |
| **Meritocracy**    | Influence is earned through meaningful contribution — not investment or marketing power.                                  |
| **Consensus**      | Major changes require multi-party consensus. No individual or company can unilaterally dictate standards.                 |
| **Neutrality**     | The governance structure exists to maintain integrity, not to profit. The Foundation acts as steward, not owner.          |
| **Open Licensing** | All canonical documents are shared under **CC BY-SA 4.0**, ensuring perpetual openness and attribution continuity.        |

---

## 3. Governance Structure

```
UXHuman Ecosystem
│
├── 1️⃣ UXHuman Foundation (Stewardship)
│     • Holds canonical documents and public repositories
│     • Safeguards licensing, attribution, and compliance with values
│     • Coordinates versioning and releases
│
├── 2️⃣ Working Groups (Evolution)
│     • UX & Design WG → evolves the Playbook and UI standards
│     • Architecture WG → evolves protocols and schemas
│     • Economic WG → evolves incentive models
│     • Compliance WG → governs certification and regulation alignment
│
├── 3️⃣ Contributor Network (Execution)
│     • Designers, developers, researchers, institutions
│     • Submit contributions via open Git-based repositories
│
└── 4️⃣ Certification Council (Quality)
       • Defines and audits certification programs
       • Oversees public registry of certified IntentSites
       • Ensures integrity of marks and KPIs
```

The **Foundation Board** acts as temporary steward until the first democratic elections after ecosystem adoption (no later than 18 months post-release).

---

## 4. Decision & Voting Model

### 4.1 Decision Levels

| Level        | Scope                                                   | Decision Body                      | Process                                      |
| ------------ | ------------------------------------------------------- | ---------------------------------- | -------------------------------------------- |
| **Minor**    | Typo fixes, small clarifications                        | Document maintainers               | Direct commit                                |
| **Moderate** | New examples, KPIs, or diagrams                         | Relevant Working Group             | Consensus + pull request                     |
| **Major**    | New principles, architecture patterns, governance rules | UXHuman Foundation + Public Review | 30-day RFC, community discussion             |
| **Critical** | Licensing, ownership, or value changes                  | Full ecosystem vote                | 2/3 supermajority of registered contributors |

### 4.2 Contributor Registration & Voting Rights

**Who Can Vote**

| Category                      | Eligibility                               | Votes                                           |
| ----------------------------- | ----------------------------------------- | ----------------------------------------------- |
| **Individual Contributor**    | ≥3 merged contributions in past 12 months | 1 vote                                          |
| **Institutional Contributor** | Certified IntentSite + active membership  | 5–15 votes (size-based, capped at 30% of total) |
| **Working Group Lead**        | Elected annually by WG peers              | 1 vote                                          |
| **Foundation Board**          | Veto only if core values violated         | No direct voting on RFCs                        |

**Anti-Capture Rules**

- No entity may control >15% of total votes.
    
- Excess institutional votes redistributed to individual contributors.
    
- Anonymous voting allowed to prevent pressure or coordination.
    

**Registration**

1. Create account at [uxhuman.org](https://uxhuman.org/)
    
2. Link to contribution repository (GitHub/GitLab)
    
3. Automatic tracking activates voting rights
    

---

### 4.3 Change Process

```
1. Contributor submits RFC → /rfc directory
2. Working Group review and refinement
3. Public discussion (30 days)
4. Vote or consensus validation
5. Merge into next document version
```

---

### 4.4 Protocol Evolution

The **/intent** endpoint and **Intent Manifest schema** follow strict versioning and open RFC procedures.

|Type|Process|Approval|
|---|---|---|
|**Non-breaking** (additive)|WG consensus + immediate adoption|Architecture WG|
|**Breaking** (incompatible)|Public RFC (60-day discussion) + 2/3 majority vote|Ecosystem vote|

**Versioning**

- Current version: `/intent/v1`, `intentmanifest.yaml` v1.0
    
- New major versions must include reference implementation and backward-compatibility plan.
    
- Deprecated versions remain supported for at least 18 months or until <5% usage.
    

---

## 5. Certification Framework (Linked to UXHuman KPIs)

Certification ensures fidelity to the UXHuman values and verifiable performance on Playbook KPIs.

### 5.1 IntentSite Verified

|Tier|Language Index|Trust Index|Mental Load Index|Sovereignty Index|Overall Score|
|---|---|---|---|---|---|
|**Baseline**|≥75|≥80|≥78|≥90|≥80|
|**Advanced**|≥85|≥90|≥88|≥95|≥90|
|**Exemplary**|≥95|≥95|≥95|≥98|≥95|

### 5.2 Audit Process

1. **Self-Assessment** — Company runs KPI suite locally.
    
2. **Automated Audit** — Tools verify performance and compliance.
    
3. **Human Validation** — UX and Compliance WGs review user-level behavior.
    
4. **Certification Decision** — Council issues badge + public record.
    
5. **Annual Renewal** — Re-audit within 12 months.
    

### 5.3 Public Registry

Ask for certified IntentSites at [uxhuman.org](https://uxhuman.org) with:

- Tier level
    
- Overall and per-index scores
    
- Audit date and validity period
    

---

## 6. Economic Sustainability

Governance must fund itself sustainably while remaining non-profit and transparent.

### 6.1 Revenue Sources

To sustain operations while remaining non-profit and transparent, the UXHuman Foundation may derive revenue from the following sources:

- **Certification Fees**
  - Collected from organizations seeking UXHuman or IntentSite certification.
  - Scaled according to company size and certification scope.
  - Fees fund audits, infrastructure, and open-source tooling.

- **Membership Contributions**
  - Individual contributors participate freely.
  - Institutional and corporate members contribute annual dues aligned with their size and engagement level.
  - Membership tiers and benefits (advisory access, early RFC participation, discounts) are reviewed annually.

- **Training & Workshops**
  - Optional courses and bootcamps supporting UXHuman adoption.
  - Revenue reinvested in community education and tooling.

- **Public Grants & Partnerships**
  - Collaboration with governments, academic institutions, and digital-inclusion programs.
  - Grants dedicated to accessibility, research, and ecosystem growth.

> All rates and tiers are determined annually by the Foundation Board in consultation with the community, published in a separate **Funding Policy Document**, and subject to public audit.


### 6.2 Allocation of Funds

The UXHuman Foundation allocates its resources to sustain openness, quality, and accountability across the ecosystem.  
Typical allocation priorities include:

- **Open-source tooling and reference implementations** — maintaining free access to core assets and documentation.
- **Working-group support** — providing stipends, tools, and operational resources for active contributors.
- **Certification and audit programs** — ensuring independence, quality, and neutrality in compliance assessments.
- **Foundation operations** — covering legal, accounting, and transparency processes required for governance integrity.

Actual percentages are reviewed annually by the Foundation Board and published in the *Annual Financial Report* after independent audit.

### 6.3 Transparency

- Quarterly financial reports published openly.
    
- Annual audit by independent accounting firm.
    

---

## 7. Enforcement & Brand Protection

### 7.1 Misuse Prevention

**Prohibited:**

- Claiming “IntentSite Verified” or “UXHuman Compliant” without certification.
    
- Misrepresenting or violating the applicable UXHuman open-license terms (CC BY 4.0 / CC BY-SA 4.0) or using trademarked material outside authorized scope.

UXHuman™, IntentWeb™, and IntentSite™ are registered or claimed trademarks of Smart Goldfish SAS. Unauthorized commercial or misleading use of these marks is prohibited.

### 7.2 Enforcement Steps

- **Warning** — Formal notice and opportunity to correct.
    
- **Delisting** — Removal from certification registry.
    
- **Public Disclosure** — Publication of a compliance notice outlining the issue and corrective steps taken.
    
- **Legal Action** — Trademark or license enforcement (last resort).
    

### 7.3 License Compliance

- Where the governing license includes a ShareAlike clause, derivative works must remain open under the same terms.
- Violations are handled via public reporting and audit review.

---

## 8. Metrics for Governance Health

To ensure transparency and continuous improvement, the UXHuman Foundation tracks a set of governance-health indicators reviewed each quarter.

| Metric | Direction | Purpose |
| --- | --- | --- |
| **Contributor Growth** | Sustained annual increase | Measures openness and community vitality |
| **Certification Adoption** | Steady year-over-year growth | Tracks ecosystem engagement and trust |
| **Financial Transparency Index** | Full public reporting | Ensures fiscal accountability |
| **Geographical Diversity Index** | Representation across ≥ 4 regions | Prevents regional or institutional capture |
| **Governance Response Time** | Timely resolution of proposals | Measures process efficiency |
| **System Resilience Index** | ≥ 99 % uptime of public repositories | Ensures reliability and continuity |

All metrics are derived from publicly verifiable repository data, certification records, and financial disclosures.  
Quarterly reports are published on the governance site and archived for long-term access.

---

## 9. Dispute Resolution & Ethics

The UXHuman ecosystem maintains a clear and transparent pathway for resolving conflicts and ethical concerns.

1. **Working Group Mediation** — First line of resolution for contributor or process disputes.  
2. **Foundation Ethics Review** — Activated when mediation fails or systemic issues arise.  
   - Composed of three rotating members: one Foundation Board delegate, one Working Group lead, and one independent community representative.  
3. **Appeal Process** — Parties may appeal an Ethics Review decision within 30 days by written request to the Foundation Board.  
4. **Public Report** — Summary of all formal disputes and resolutions is published quarterly for transparency.  
5. **Code of Conduct Enforcement** — Zero tolerance for harassment, discrimination, or manipulation. All contributors agree to uphold the **UXHuman Code of Conduct** upon participation.

**Jurisdiction** — Unless otherwise required by law, all disputes are governed by French law, with exclusive jurisdiction in the Tribunal de Commerce de Nice.  

---

## 10. Closing Statement

> The IntentWeb is not owned — it is *stewarded.*  
> UXHuman is not a brand — it is a covenant between creators and users to build a Web that speaks human, acts with trust, and honors sovereignty.  

This Governance Framework ensures that covenant remains transparent, accountable, and alive.  

**Adopted by the UXHuman Foundation under the stewardship of Smart Goldfish SAS — effective May 2025.**

---

© 2025 UXHuman Initiative / Smart Goldfish SAS
Licensed under the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).  
If you remix or build upon this material, you must distribute your contribution under the same license,  
with attribution to “UXHuman (www.uxhuman.org)”.  
See the full license at https://creativecommons.org/licenses/by-sa/4.0/

Visual assets (logos, icons, and illustrations) are excluded from this license and remain the property of Smart Goldfish SAS.  
UXHuman™, IntentWeb™, and IntentSite™ are registered or claimed trademarks of Smart Goldfish SAS.  
Unauthorized commercial or misleading use of these marks is prohibited.
