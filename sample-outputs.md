# Sample Agent Outputs

These are **actual, unedited responses** produced by the Grounding Integrity Sentinel agent (Microsoft Copilot Studio, Work IQ enabled, Claude Opus 4.5) when run against the Meridian Cloud knowledge base. They are included as evidence of the agent's real reasoning behavior. The agent was never told where the contradictions were; it located and reasoned about them from the documents alone.

Each output below maps to one of the four planted contradictions described in the README.

---

## Output 1 — Cross-silo authority conflict (Contradiction 2)

**Question asked:** *"Can an Account Executive approve a 20% discount on their own?"*

**Agent response (summary of the captured output):**

The agent identified a genuine conflict between two documents:

- `Sales-Playbook-Discounting.docx` — *"Account Executives may approve discounts of up to 25% off list price on their own authority, without escalation."*
- `Deal-Desk-Approval-Matrix.docx` — *"Self-approval authority for Account Executives ends at 15%. Where a departmental playbook states a higher self-approval limit, this matrix governs."*

**Canonical source chosen:** the Deal Desk Approval Matrix, with reasons given in priority order — (1) classification (Controlled outranks Internal), (2) explicit precedence statement ("It supersedes any conflicting approval thresholds stated in departmental playbooks"), (3) effective date as a tiebreaker only.

**Correct answer returned:** *No.* A 20% discount falls in the "Above 15% up to 30%" band, which requires Deal Desk (Finance) approval.

**Recommended action:** align the Sales Playbook's AE self-approval threshold from 25% down to 15% to match the governing Deal Desk matrix — preserving the playbook's other guidance, changing only the conflicting number. Recommended for owner approval; no automatic change.

---

## Output 2 — Recency trap (Contradiction 1)

**Question asked:** *"What is our guaranteed Priority-1 response time for Enterprise customers?"*

**Agent response (summary of the captured output):**

The agent identified a high-severity conflict:

- `Enterprise-MSA-Template.docx` (Controlled — Contractual) — *"The Priority-1 first-response commitment for Enterprise customers is one (1) hour, twenty-four hours a day, seven days a week."*
- `Enterprise-Support-Runbook.docx` (Internal, "draft, recently published") — lists P1 target first response as **4 hours**.

**Canonical source chosen:** the MSA, because (1) it is Controlled–Contractual and states its commitments take precedence over internal operational guidance, (2) the Support Tier Definitions document corroborates that the signed agreement governs, and (3) the runbook itself is still labelled "draft." The agent explicitly noted that the *newer* document was the *wrong* one — weighing authority over recency.

**Correct answer returned:** 1 hour, 24×7, with a 10% monthly-fee service credit if missed.

**Recommended action:** update the runbook's response targets to match the contractual floor and add a cross-reference to the MSA; consider removing the "draft" label. Preserve the runbook's operational content. Recommended for owner approval.

---

## Output 3 — Reconcilable false-positive — the restraint case (Contradiction 3)

**Question asked:** *"How long do we keep an EU customer's personal data after they close their account?"*

**This is the agent's most important behavior.** A naive detector would flag "7 years vs 30 days" as a contradiction and recommend a fix — and either fix would cause harm (destroying legally required records, or stating a GDPR violation). The Sentinel instead recognized the two rules as **reconcilable** and refused to recommend any change.

**Agent response (key passages from the captured output):**

- It identified the two claims (Global policy: personal-data default life-of-account + 90 days; EU Addendum: erase EU personal data within 30 days) and concluded **"No Genuine Conflict."**
- Reasoning: the documents are *deliberately scoped to different jurisdictions and data categories*, and explicitly cross-reference each other ("the EU Data Processing Addendum... override this default for EU data subjects").
- **Correct answer returned:** for an EU customer, personal data is deleted within 30 days of account closure; financial transaction records are retained 7 years separately, as a distinct data category.
- **Restraint, stated explicitly:** *"Because these documents govern different jurisdictions and data categories, I am not recommending any deletion, overwriting, or consolidation. Doing so could destroy legally required distinctions and cause a regulatory breach."*
- **Recommendation:** no document changes required.

---

## Output 4 — Structured vs prose (Contradiction 4)

**Question:** *"What is the monthly list price of the Enterprise tier?"*

**Expected behavior:** the agent reconciles `Pricing-Tiers` (authoritative rate card, $4,000) against `Pricing-FAQ` (indicative, "from $3,500"), and returns $4,000 as the source-of-truth figure because the rate card declares itself authoritative and the FAQ defers to it.

---

*All outputs above were produced on synthetic data (the fictional "Meridian Cloud"). No real or proprietary company data was used.*
