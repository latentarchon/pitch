# Latent Archon — Concept Paper

**Need-to-Know AI-Powered Document Search for Government**

*UNCLASSIFIED — Approved for public distribution*

---

## The Problem

Federal agencies, the Department of Defense, and law enforcement organizations manage enormous volumes of sensitive documents — contracts, technical manuals, policy memoranda, intelligence products, case files, after-action reports — spread across file shares, SharePoint sites, and legacy systems. Finding the right information requires analysts to know which document exists, where it lives, and how to search for it using exact keywords.

This creates three compounding failures:

1. **Knowledge loss at scale.** When experienced personnel rotate, deploy, or separate, their knowledge of what documents exist and where to find them leaves with them. Institutional memory becomes a personnel dependency.

2. **Unauthorized AI usage.** Personnel under time pressure turn to consumer AI tools — ChatGPT, Microsoft Copilot, Google Gemini — to work faster. They paste CUI, FOUO, and law enforcement sensitive content into systems with no authorization boundary, no audit trail, and no data residency guarantee. This is not a hypothetical risk; it is happening today across the federal workforce.

3. **Need-to-know violations in search.** Existing enterprise search tools return results based on keyword matches across everything a system administrator has indexed. They do not enforce need-to-know at the query level. An analyst searching a shared repository may see document titles, snippets, or metadata for programs they are not authorized to access — or worse, miss documents they *are* authorized for because those documents live in a different system entirely.

The result: analysts either spend hours searching manually, miss critical source material, or bypass security controls entirely to get answers faster.

---

## The Solution

Latent Archon is a document intelligence platform purpose-built for government agencies handling sensitive information. Analysts upload documents into access-controlled workspaces, then ask questions in natural language. The platform returns cited, source-traceable answers drawn exclusively from documents the analyst is authorized to see.

**Three capabilities define the platform:**

### 1. Need-to-Know Enforcement by Design

Latent Archon enforces need-to-know at every layer of the system — not as an afterthought bolted onto a general-purpose search engine, but as the foundational architecture:

- **Workspace isolation.** Documents are organized into workspaces scoped by program, mission, case, team, or classification level. Each workspace has its own membership roster and role assignments. Analysts see only the workspaces they have been explicitly granted access to.

- **Three-layer data isolation.** Even if the application layer has a defect, data cannot leak across boundaries:
  1. **Database-level tenant isolation** — each organization's data is partitioned at the infrastructure layer
  2. **PostgreSQL row-level security (RLS)** — every database query is scoped to authorized workspace IDs before execution; if no workspace is set, zero rows are returned (fail-closed)
  3. **Workspace-level RBAC** — membership, roles, and permissions are controlled per workspace by program managers or administrators

- **Selective cross-workspace search.** Analysts choose which of their authorized workspaces to include in a query. A contracting officer can search across 12 program workspaces simultaneously; an analyst in a compartmented program searches only that workspace. The system never returns results from workspaces the user is not a member of.

- **Vector store isolation.** Even the AI search index enforces workspace boundaries — every stored embedding carries workspace and document namespace restrictions, preventing cross-workspace data leakage at the vector database level.

### 2. AI-Powered Semantic Search with Source Citations

Latent Archon uses retrieval-augmented generation (RAG) to answer questions across documents:

- **Semantic understanding.** The search engine understands meaning, not just keywords. A query for "What are the maintenance intervals for the APU?" finds the answer even if the document says "auxiliary power unit scheduled servicing." Analysts no longer need to guess the exact terminology a document author used.

- **AI-synthesized answers with full citations.** The platform synthesizes information across multiple documents and presents a structured answer. Every claim in every response cites the specific document, page, and passage it was derived from. Analysts verify before acting — the system is a research accelerator, not an oracle.

- **Conversational refinement.** Analysts ask follow-up questions that narrow or expand the search. The platform maintains conversation context, enabling iterative analysis: "Which of those modifications were sole-source?" or "Show me the contradictions between these two assessments."

- **Multi-format document support.** PDF, DOCX, PPTX, XLSX, images (with OCR), and plain text. Microsoft 365 integration automatically syncs documents from SharePoint sites with delta-based incremental updates.

### 3. Government-Grade Security Without Government-Grade Friction

The platform is designed for agencies that handle CUI, FOUO, CJIS, and DoD mission data:

- **FedRAMP High aligned, IL5 deployed.** Runs on Google Cloud Platform Assured Workloads in us-east4 (Northern Virginia) with US-only data residency and US-person-only personnel controls.

- **Customer-managed encryption.** All data encrypted at rest with AES-256 using FIPS 140-2 Level 3 HSM-backed keys. Per-tenant encryption keys enable crypto-shredding — destroy the key, destroy the data, verifiably and permanently.

- **DLP and malware scanning.** Every uploaded document is scanned for PII, credentials, and malware before entering the search index. Sensitive content is flagged or redacted before indexing.

- **Zero AI training on government data.** Documents are embedded for search retrieval only. No model is fine-tuned, retrained, or updated using government data. Data never leaves the authorization boundary.

- **Immutable audit trail.** Every document access, search query, and AI response is logged with user identity, timestamp, and client IP. Audit data is available for export to agency SIEM platforms.

- **Self-service deployment.** Agencies can begin a 30-day pilot in days — upload documents, configure SSO, start querying. No multi-million-dollar integration, no vendor engineering team required, no months-long onboarding.

---

## How It Works

**Scenario:** A DoD program manager needs to review 3 years of contract modifications across 12 programs to brief leadership on cost growth trends.

1. **Documents are already organized.** Contract folders are uploaded into program-specific workspaces. New modifications sync automatically from SharePoint overnight.

2. **The PM asks a question.** She selects all 12 program workspaces and asks: *"Summarize all contract modifications that increased cost by more than 10%. Group by program and identify the stated justification for each increase."*

3. **The platform answers with citations.** Latent Archon finds 47 relevant modifications across the 12 programs, generates a structured summary grouped by program, and cites the specific modification number, page, and passage for each entry.

4. **She verifies and refines.** She clicks a citation to view the original document in context, then asks: *"Which of these were sole-source? What was the J&A rationale?"* The platform narrows the search and returns cited answers.

**What previously took 2-3 days of manual document review takes 15 minutes.**

This scenario generalizes across every document-heavy workflow in government: technical manual lookup, policy compliance review, intelligence synthesis, case file analysis, lessons learned search, and regulatory cross-referencing.

---

## Differentiation

| | **Latent Archon** | **General-Purpose AI (ChatGPT, Copilot)** | **Enterprise Search (SharePoint, Google Workspace)** | **Large Platform (Palantir AIP)** |
|---|---|---|---|---|
| **Need-to-know enforcement** | Three-layer isolation: database tenant, RLS, workspace RBAC | None — data leaves the boundary | Coarse-grained folder permissions | Configurable, requires vendor professional services |
| **Source citations** | Every response cites exact document and passage | Hallucination risk, no traceable sourcing | Returns document links, not answers | Varies by configuration |
| **CUI/FOUO compliance** | FedRAMP High aligned, IL5, CMEK, DLP, immutable audit | Not authorized for CUI | Platform-dependent | FedRAMP authorized |
| **AI training on your data** | Never — zero fine-tuning on government data | Terms of service vary; data may be used for training | N/A (no AI synthesis) | Platform-dependent |
| **Time to value** | Days (upload docs, start querying) | Immediate but unauthorized | Weeks (if already deployed) | Months to years, $5M-$50M+ |
| **Cost** | Per-org subscription, infrastructure included | Per-user, but unauthorized for sensitive data | Bundled with platform licensing | Multi-million-dollar contracts |

---

## Security Posture Summary

| Domain | Implementation |
|--------|---------------|
| Authorization framework | FedRAMP High aligned, IL5 Assured Workloads, CJIS v5.9.5 mapped |
| Encryption at rest | AES-256-GCM, CMEK with FIPS 140-2 Level 3 HSM, per-tenant key isolation, crypto-shredding |
| Encryption in transit | TLS 1.3, HSTS with 2-year preload |
| Authentication | SAML 2.0 SSO (CAC/PIV compatible), TOTP MFA, SCIM 2.0 provisioning |
| Session management | Configurable idle/absolute timeouts, concurrent session limiting |
| Data protection | DLP scanning, malware scanning, document-level permissions |
| Audit | Immutable audit logs, SIEM export, OpenTelemetry tracing |
| Infrastructure | GCP Assured Workloads, VPC-isolated, Private Service Connect, egress firewall (deny-all default) |
| Supply chain | SBOM generation, container image signing (Cosign/Sigstore), distroless images, FIPS 140-2 Go binary |
| Adversarial testing | 99 automated red team attacks across 6 suites, MITRE ATT&CK mapped |

---

## Deployment Options

| Tier | Domain | Status | Compliance | Audience |
|------|--------|--------|------------|----------|
| **Federal** | `fed.latentarchon.com` | Active (staging deployed) | FedRAMP Moderate (in progress) | Federal agencies, DoD, IC |
| **Air-Gapped** | Customer-controlled | Prototype | DISA PA (planned) | Classified programs |
| **SLTT** | `gov.latentarchon.com` | Planned | StateRAMP (planned) | State, local, tribal agencies |
| **Commercial** | `cloud.latentarchon.com` | Planned | SOC 2 Type II (planned) | Private sector |

All tiers run the same codebase with the same security baseline. The air-gapped prototype deploys via Helm charts to disconnected infrastructure with CAC/PIV mTLS authentication.

---

## Company

**Latent Archon, Inc.** was founded to close the document intelligence gap in government. The platform — backend, frontend, infrastructure, security architecture, and compliance automation — was built entirely with internal capital. No venture funding. No dependency on external engineering teams.

- **Founder:** Andrew Hendel (Stanford BS/MS Engineering, Phi Beta Kappa, Tau Beta Pi). 15 years managing $1B+ in institutional capital. Multiple technology startup foundations. Architect of the full Latent Archon platform.
- **Strategic Advisor:** Nathan Mintz. Co-founder and former CEO of Epirus (defense unicorn, $250M+ raised, production Northrop Grumman contracts). 14 years as EW/radar systems engineer at Raytheon and Boeing.

---

## Next Steps

1. **See a demo.** Schedule a live demonstration at [latentarchon.com](https://latentarchon.com) or contact@latentarchon.com.
2. **Start a pilot.** 30-day evaluation scoped to a single office, program, or unit. SSO integration and analyst training included.
3. **Review our security posture.** Full compliance documentation, security whitepaper, and CJIS compliance mapping available at [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance).

---

*Latent Archon, Inc. | April 2026*
*Document ID: CONCEPT-001 | Version: 1.0*
*Contact: contact@latentarchon.com*
