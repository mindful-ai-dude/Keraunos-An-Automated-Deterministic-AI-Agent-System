### Keraunos: An Automated Human-In the Loop Deterministic AI Agent System
![Keraunos Logo](images/v1_keraunos_ai_logo.jpg)

## The Synthesis: Law Firm Use Case 

![Legal Use Case](images/legal_use_case.png)

A partner types in a "Legal Document Agent" USE CASE — for example, "Draft an NDA for a SaaS vendor handling PHI data, governed by California law, with a 2-year term and mutual non-disclosure obligations." The system — powered by a Goose-based agent runtime — uses the Blueprint Pipeline (Stripe pattern) where deterministic nodes handle jurisdiction detection, regulatory clause extraction, template compliance validation, citation verification, conflict-of-interest scanning, and privilege-safe audit logging, while non-deterministic nodes (bounded LLM calls) handle clause drafting, risk analysis narrative, and plain-language summary generation. 

Context is served via a centralized MCP Toolshed pre-hydrated with the firm's clause library, matter management data, jurisdictional requirements, and regulatory databases. The system is guided by an AGENTS.md file (passive context encoding the firm's style guide, citation format, and ethical rules), conditional rule files (.goosehints for practice-area-specific constraints), and modular Skills (activated on demand for specialized tasks like HIPAA mapping or SEC filing review). 

Output: a complete, jurisdiction-verified, citation-checked legal document with a compliance certification, a redline showing all AI-generated content, and a risk assessment — all produced under a deterministic audit trail that satisfies bar association requirements for AI-assisted legal work. The firm's custom fine-tuned model (distilled from a 14B Teacher to a 3B Student using the firm's own approved precedent library) ensures terminology, tone, and formatting match the firm's existing work product exactly. Self-hosted on the firm's own infrastructure — no client data ever leaves the firm's network.

---
## 1. The Problem: Why Law Firms Need Keraunos

### 1.1 The Document Volume Crisis

A mid-size law firm (50-200 attorneys) produces 10,000-50,000 documents annually: contracts, NDAs, corporate governance documents, regulatory filings, litigation briefs, opinion letters, due diligence reports, and client memoranda. The economics are brutal:

- **Junior associate time:** $250-450/hour spent on first drafts that partners rewrite
- **Average NDA:** 3-5 billable hours to draft, review, and finalize ($750-$2,250)
- **Complex contract:** 20-40 billable hours ($5,000-$18,000)
- **Due diligence report:** 80-200 billable hours ($20,000-$90,000)
- **Regulatory filing:** 40-100 billable hours ($10,000-$45,000)

A significant portion of this work is structurally repetitive. The same clauses appear across thousands of documents with jurisdiction-specific variations. The legal reasoning is specialized, but the assembly is mechanical.

### 1.2 Why Current AI Tools Fail Law Firms

Every major AI vendor is pitching "AI for legal." They all fail on the same points:

**No Compliance Verification.** ChatGPT, Copilot, and Harvey generate plausible legal text. But "plausible" is not "correct." No existing tool deterministically verifies that a generated contract includes all jurisdiction-required clauses, that citations reference actual case law, or that regulatory references are current. A partner must still read every word — eliminating the productivity gain.

**No Audit Trail.** Bar associations increasingly require disclosure of AI assistance in legal work. Current tools provide no immutable record of what the AI generated versus what a human wrote. This creates ethical exposure.

**No Data Sovereignty.** Law firms have ethical obligations to protect client confidentiality (ABA Model Rule 1.6). Sending client data to OpenAI, Google, or Microsoft cloud endpoints — even with enterprise agreements — creates risk that many firms are unwilling to accept. Privilege waiver arguments are untested.

**No Firm-Specific Knowledge.** Every firm has its own clause libraries, formatting standards, citation preferences, and institutional knowledge. Generic AI models produce generic output that requires extensive rework to match firm standards.

### 1.3 What Keraunos Solves

Keraunos addresses every failure point:

| Problem | Keraunos Solution |
|---------|------------------|
| No compliance verification | Deterministic nodes verify jurisdiction requirements, citation accuracy, and regulatory completeness — the AI cannot bypass these checks |
| No audit trail | Every pipeline run produces an immutable, timestamped log of every node execution, every AI decision, and every human review — stored in PostgreSQL |
| No data sovereignty | Fully self-hosted on the firm's own infrastructure. Client data never leaves the firm's network. Air-gappable. No external API calls required. |
| No firm-specific knowledge | Custom fine-tuned models distilled from the firm's own precedent library using the Keraunos Distillation System (14B Teacher to 3B Student) |
| No quality guarantee | 10-stage pipeline with deterministic quality gates. Max 2 AI fix attempts before human escalation. |

---
## 2. The Keraunos Legal Pipeline: 10 Stages

![Legal Use Case Pipeline](images/legal_use_case_pipeline.png)

When a legal professional triggers Keraunos (typing `/legal Draft an NDA for...`), the following 10-stage pipeline executes:
![AI Sequential AI Legal Pipeline](images/legal_pipeline_diagram_color.png)
---

### Stage Details

**Stage 1: Matter Intake & Classification (Deterministic)**
MCP tools parse the request and extract: document type (NDA, MSA, SPA, brief, memo), jurisdiction (state, federal, international), practice area (corporate, IP, employment, litigation, regulatory), parties and roles, key commercial terms, and governing law. This is pure pattern matching and entity extraction — no LLM creativity needed.

**Stage 2: Document Architecture (Non-Deterministic, Bounded)**
The LLM — using the firm's custom fine-tuned model — designs the document structure: which sections to include, which clauses are required vs optional for this jurisdiction, which firm templates to reference. Output is structured JSON validated against the firm's document schema.

**Stage 3: Structure Validation (Deterministic)**
Deterministic check: Does the planned structure include all jurisdiction-required sections? California NDAs require specific trade secret definitions per Civil Code §3426.1. Delaware corporate documents require specific statutory references. HIPAA-adjacent agreements require BAA provisions. This check is a lookup table, not LLM reasoning — it cannot be wrong.

**Stage 4: Clause Generation (Non-Deterministic, Bounded)**
The LLM drafts each clause using: the firm's clause library (retrieved via MCP `search_clause_library` tool), jurisdiction-specific language from the regulatory database, and the custom fine-tuned model that has learned the firm's exact tone, formatting, and citation style through distillation.

**Stage 5: Clause Fix Loop (Non-Deterministic, Max 2 Rounds)**
If the compliance scan (Stage 6) or citation verification (Stage 7) finds issues, the LLM attempts to fix them. Maximum 2 attempts. After 2 failures, the document is marked `ESCALATED` and routed to a human attorney with specific notes on what failed and why. The AI never silently ships a non-compliant document.

**Stage 6: Compliance Scan (Deterministic)**
Automated checks against: jurisdiction-specific clause requirements, regulatory reference currency (are cited regulations still in effect?), internal consistency (do defined terms match across sections?), conflicting provisions, and missing boilerplate. This is the "legal linter" — it checks the document against rules the same way a code linter checks source against style rules.

**Stage 7: Citation Verification (Deterministic)**
Every case citation is checked: Does this case exist? Is it still good law (not overruled or superseded)? Is the citation format correct per the firm's style guide (Bluebook, ALWD, or firm-specific)? Every statute reference is validated against the current code. Every regulatory reference is checked for currency. This is deterministic database lookup — no LLM reasoning, no possibility of hallucination.

**Stage 8: Risk Assessment (Non-Deterministic, Bounded)**
The LLM generates a risk analysis: identifies terms unfavorable to the client, missing protections, liability exposure areas, and comparison to market standard terms. This is the one stage where LLM creativity adds genuine value — pattern recognition across thousands of similar agreements.

**Stage 9: Conflict & Privilege Check (Deterministic)**
Deterministic check against the firm's matter management database: Are any parties in this transaction adverse to existing clients? Does the generated document contain any information that could waive attorney-client privilege? Are all AI-generated sections clearly marked for the audit trail?

**Stage 10: Output Packaging (Deterministic)**
Produces:
- **The document** in the firm's standard format (DOCX with firm letterhead)
- **A redline** showing all AI-generated content vs template content
- **A risk assessment memo** with specific recommendations
- **A compliance certification** listing every check passed
- **An immutable audit trail** (timestamped log of every stage, every AI decision, every human review, stored in PostgreSQL)

---

## 3. Legal-Specific MCP Tools (The Legal Toolshed)

In addition to the 64 general-purpose tools, Keraunos for Law Firms adds 20 legal-specific MCP tools:

### Legal Document Tools (8)

| Tool | Purpose |
|------|---------|
| `search_clause_library` | Semantic search across the firm's clause library (pgvector) |
| `validate_jurisdiction` | Check document against jurisdiction-specific requirements |
| `verify_citation` | Validate case citations (existence, good law status, format) |
| `check_statute_currency` | Verify statute references are current and not amended |
| `detect_conflicting_clauses` | Find internally contradictory provisions |
| `extract_defined_terms` | Parse all defined terms and check consistency |
| `generate_redline` | Produce tracked-changes version showing AI vs template content |
| `format_document` | Apply firm's formatting standards (fonts, margins, numbering) |

### Compliance & Regulatory Tools (6)

| Tool | Purpose |
|------|---------|
| `map_regulatory_requirements` | Map document to applicable regulations (HIPAA, GDPR, SOX, etc.) |
| `check_regulatory_coverage` | Verify all required regulatory provisions are included |
| `scan_privilege_risk` | Detect content that could waive attorney-client privilege |
| `check_ethical_compliance` | Verify against ABA Model Rules and state bar requirements |
| `generate_ai_disclosure` | Create required disclosure of AI assistance per bar rules |
| `check_data_sovereignty` | Verify no client data references external services |

### Matter Management Tools (6)

| Tool | Purpose |
|------|---------|
| `check_conflicts` | Query matter DB for client/party conflicts |
| `log_ai_action` | Record AI decision to immutable audit trail |
| `retrieve_precedent` | Find similar past documents from firm's DMS |
| `calculate_billing_impact` | Estimate time saved vs manual drafting |
| `route_for_review` | Assign document to appropriate partner for human review |
| `generate_matter_memo` | Create matter file entry documenting AI-assisted work |

---

## 4. Custom Fine-Tuned Legal Model (The Distillation System)

This is where Keraunos becomes truly specific to a law firm. Using the Keraunos AI Custom Fine-Tuning Distillation System:

### Phase 1: Build the Golden Dataset from Firm Precedent

The firm provides its approved precedent library — the best examples of each document type as produced by the firm's partners over the years. These become the "Golden Dataset." Total exemplar count ranges from 1,000 for a boutique practice to 50,000+ for a multi-practice, multi-jurisdictional firm.

- **NDAs:** 200-5,000 approved exemplars across jurisdictions
- **MSAs/SOWs:** 150-3,000 approved exemplars
- **Corporate governance:** 100-2,000 approved exemplars
- **Regulatory filings:** 100-5,000 approved exemplars per filing type
- **Client memos:** 200-10,000 approved exemplars
- **Litigation documents:** 200-15,000 approved exemplars (briefs, motions, discovery)
- **Opinion letters:** 50-5,000 approved exemplars

### Phase 2: Author the Product Policy (Legal Style Guide)

A 20-page document encoding the firm's exact standards:

- **Citation format:** Bluebook 21st Ed with firm-specific modifications
- **Defined term conventions:** capitalization, placement, cross-reference style
- **Clause ordering:** the firm's preferred section sequence per document type
- **Tone and formality:** the specific register the firm uses with each client type
- **Boilerplate preferences:** which standard clauses the firm always includes
- **Redline conventions:** how tracked changes are formatted and presented

### Phase 3: Synthetic Data Generation

Using the Golden Dataset and Product Policy, a SOTA frontier model generates 10,000-500,000 synthetic training pairs (10x multiplier on the Golden Dataset):
- Input: "Draft a non-compete clause for a California employment agreement, 12-month term, limited to direct competitors"
- Output: The exact clause the firm would produce, in the firm's format, with the firm's preferred language

Human review gate: attorneys verify a sample before inclusion.

### Phase 4: Multi-Tier Distillation

| Model | Parameters | Purpose | Cost/Token |
|-------|-----------|---------|------------|
| Teacher | 14B-20B | Learn the firm's complete legal style and knowledge | High (training only) |
| Intermediate | 7B-10B | Bridge model for compression | Medium (training only) |
| Student | 3B-4B | Production inference model | Very low (~$0.001/1K tokens) |

The 3B Student model runs locally on the firm's own hardware via Ollama. Inference cost is effectively zero after training. The model produces output that is stylistically indistinguishable from the firm's own attorneys' work product.

### Phase 5: Continuous Learning

As partners review and modify AI output, those corrections feed back into the training pipeline. The model improves with every document the firm produces. After 6 months of production use, the model has learned from thousands of partner corrections — knowledge that would take a junior associate years to accumulate.

---

## 5. Data Sovereignty and Ethical Compliance

### Self-Hosted Architecture

The entire Keraunos system runs on the firm's own infrastructure:

- **VPS or on-premise server** (32GB+ RAM recommended)
- **Dokploy** manages containerized services
- **PostgreSQL + pgvector** stores all data on-premise
- **Ollama** runs the custom fine-tuned model locally
- **No external API calls** — the 3B Student model runs entirely on the firm's hardware
- **Air-gappable** — can operate with zero internet connectivity

**No client data ever leaves the firm's network.** Not to OpenAI. Not to Google. Not to Microsoft. Not to any cloud provider. The firm maintains complete custody of all data at all times.

### Bar Association Compliance

- **ABA Model Rule 1.6 (Confidentiality):** Satisfied by self-hosted architecture. No third-party data transmission.
- **ABA Model Rule 1.1 (Competence):** The deterministic quality gates ensure AI output meets professional standards before human review. The system is a tool that enhances competence, not a replacement for it.
- **ABA Model Rule 5.3 (Supervisory Responsibility):** The human review gate (Stage 10) ensures an attorney reviews every document. The audit trail documents the supervision chain.
- **AI Disclosure Requirements:** The `generate_ai_disclosure` MCP tool automatically creates required disclosures per the applicable state bar's AI rules.
- **Audit Trail:** Every AI action is logged immutably in PostgreSQL. The firm can produce a complete record of what the AI generated, what the attorney modified, and what was delivered to the client.

---

## 6. Financial Impact

### Per-Document Economics

| Document Type | Manual Cost | Keraunos Cost | Savings | Time Saved |
|---------------|------------|---------------|---------|------------|
| Standard NDA | $1,500 (5 hrs) | $50 (infra + review) | 97% | 4.5 hrs |
| Complex NDA (multi-jurisdiction) | $4,500 (15 hrs) | $200 (infra + review) | 96% | 13 hrs |
| Master Services Agreement | $9,000 (30 hrs) | $500 (infra + review) | 94% | 27 hrs |
| Employment Agreement | $3,000 (10 hrs) | $150 (infra + review) | 95% | 9 hrs |
| Due Diligence Report | $45,000 (150 hrs) | $3,000 (infra + review) | 93% | 135 hrs |
| Regulatory Filing | $22,500 (75 hrs) | $2,000 (infra + review) | 91% | 67 hrs |
| Client Memorandum | $1,500 (5 hrs) | $75 (infra + review) | 95% | 4.5 hrs |

### Firm-Level Annual Impact (50-Attorney Firm)

| Metric | Before Keraunos | After Keraunos | Impact |
|--------|----------------|----------------|--------|
| Documents produced/year | 10,000 | 10,000+ | Same or higher volume |
| Average draft time | 8 hours | 45 minutes | 90% reduction |
| Annual associate hours on drafting | 40,000 | 4,000 | 36,000 hours freed |
| Associate hours redirected to high-value work | 0 | 36,000 | Revenue uplift |
| Infrastructure cost | $0 | $8,400-$31,000/year (infra + maintenance) | Minimal vs capacity freed |
| Annual cost savings (at $300/hr avg) | — | $10.8M in freed capacity | — |
| Quality incidents (compliance gaps) | ~120/year (industry avg) | ~5/year (escalated + caught) | 96% reduction |

### ROI Calculation

- **Self-hosted VPS infrastructure:** $2,500-$25,000 (one-time setup, scaling with firm size)
- **Annual maintenance:** $2,400-$6,000/year (VPS monitoring, updates)
- **Model training (one-time):** $15,000-$75,000 (distillation pipeline, scaled to exemplar volume)
- **Ongoing training refinement:** $5,000-$15,000/year (incremental learning from attorney corrections)
- **Year 1 total cost:** ~$50,000-$121,000 (depending on firm size and exemplar volume)
- **Year 1 capacity freed:** $10.8M equivalent (50-attorney firm)
- **ROI:** 89:1 to 216:1 (depending on firm size and document volume)

---

## 7. Implementation Roadmap

### Month 1: Foundation
- Deploy Keraunos infrastructure on firm's hardware
- Index the firm's clause library into pgvector
- Configure matter management database connection
- Deploy the 15 starter MCP tools + 8 legal document tools

### Month 2: Distillation
- Collect Golden Dataset from firm's precedent library (1,000-50,000 exemplars depending on firm size, practice area breadth, and jurisdictional coverage)
- Author the Product Policy (legal style guide)
- Generate synthetic training data (1,000+ pairs)
- Train Teacher model (14B), distill to Student (3B)

### Month 3: Pilot
- Deploy to 3-5 attorneys across practice areas
- Start with low-risk document types (standard NDAs, client memos)
- Human reviews every document (100% review rate)
- Corrections feed back to model refinement

### Month 4-6: Expansion
- Expand to all attorneys
- Add complex document types (MSAs, regulatory filings)
- Reduce human review to targeted review (AI-flagged sections only)
- Deploy remaining legal-specific MCP tools

### Month 7-12: Optimization
- Continuous model refinement from partner corrections
- Add practice-area-specific Skills
- Integrate with firm's DMS (iManage, NetDocuments)
- Integrate with firm's billing system for automated time capture

---

## 8. Competitive Differentiation

| Feature | Harvey AI | CoCounsel | Keraunos |
|---------|----------|-----------|----------|
| Deterministic compliance verification | No | No | Yes (pipeline gates) |
| Citation verification | Partial | Yes (limited) | Yes (deterministic, every citation) |
| Custom firm-specific model | No (generic) | No (generic) | Yes (distilled from firm's precedent) |
| Self-hosted / air-gapped | No (cloud only) | No (cloud only) | Yes (fully self-hosted) |
| Client data sovereignty | Third-party cloud | Third-party cloud | Firm's own infrastructure |
| Immutable audit trail | Partial logs | Partial logs | Full pipeline audit trail (PostgreSQL) |
| Human escalation protocol | None | None | Automatic after 2 AI fix attempts |
| Bar association AI disclosure | Manual | Manual | Automated (MCP tool) |
| Cost per document | $50-500/mo subscription | $100-250/user/mo | ~$0.05 per document (inference) |
| Custom fine-tuning | Not available | Not available | Full distillation pipeline |

---

## 9. Risk Mitigation

### What If the AI Gets Something Wrong?

This is the question every firm asks. The answer is the pipeline architecture itself:

1. **The AI cannot skip quality gates.** The deterministic nodes (Stages 1, 3, 6, 7, 9) run as fixed code. They execute the same way every time. The AI has no ability to bypass, modify, or skip them.

2. **The AI gets exactly 2 fix attempts.** If a compliance scan or citation verification fails and the AI cannot fix it in 2 rounds, the system stops and escalates to a human. It does not silently continue.

3. **Every document requires human review.** Keraunos is a force multiplier, not a replacement. The pipeline produces a draft with a compliance certification and redline. An attorney still reviews and approves before delivery.

4. **The audit trail is immutable.** If a regulatory body or court asks "did you use AI to draft this?", the firm can produce a complete, timestamped record of exactly what the AI generated, what checks it passed, and what the attorney modified.

5. **The model is the firm's own.** The custom fine-tuned model was trained on the firm's own approved precedent. It produces output that matches the firm's standards by construction, not by coincidence.

---

## 10. Summary for Law Firm Partners

**What it is:** An AI system that drafts legal documents in your firm's exact style, verifies every clause against jurisdiction requirements, checks every citation against actual case law, and produces an audit trail that satisfies bar association requirements — all running on your own hardware with zero client data leaving your network.

**What it is not:** A chatbot. A search engine. A generic AI that produces plausible-sounding legal text and hopes it is correct.

**The deterministic guarantee:** Every document passes through 10 stages. Seven of those stages are deterministic — they execute the same way every time, and the AI cannot bypass them. The three creative stages (document architecture, clause drafting, risk assessment) are bounded by strict validation on both input and output.

**The business case:** A 50-attorney firm frees approximately 36,000 associate hours annually — capacity that can be redirected to high-value client work, business development, or reduced associate burnout. Year 1 total investment: $50K-$121K depending on firm size. Year 1 ROI: 89:1 to 216:1.

**The ethical case:** Self-hosted. Air-gappable. Full audit trail. Automated AI disclosure. The system was designed from the ground up to satisfy ABA Model Rules 1.1, 1.6, and 5.3.

**The competitive case:** No other legal AI product offers deterministic compliance verification, custom fine-tuned models from the firm's own precedent, and full data sovereignty. Harvey and CoCounsel are cloud-only, generic-model products with no quality gates. Keraunos is the only system where the AI must prove its work before a human sees it.

*Keraunos does not draft documents and hope they are correct. Keraunos drafts documents and proves they are correct.*
