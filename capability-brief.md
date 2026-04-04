# Latent Archon — Government Capability Brief

**Secure Document Intelligence for Federal Agencies**

*UNCLASSIFIED — Approved for public distribution*

---

## Executive Summary

Latent Archon is a FedRAMP-ready, CUI-compliant document intelligence platform that enables federal agencies to upload, organize, and query sensitive documents using AI-powered semantic search and retrieval-augmented generation (RAG). Analysts ask questions in natural language and receive sourced, cited answers drawn exclusively from their agency's own documents — no data leaves the authorization boundary, no training on government data, and every response is traceable to its source material.

---

## The Problem

Federal agencies sit on massive repositories of documents — contracts, regulations, technical manuals, policy memos, intelligence products, after-action reports — spread across file shares, SharePoint sites, and legacy systems. Finding the right information requires analysts to know which document exists, where it lives, and how to search for it. This creates:

- **Hours lost** to manual document search across disconnected systems
- **Institutional knowledge gaps** when experienced personnel rotate or separate
- **Inconsistent analysis** when analysts miss relevant source material
- **Security risk** when personnel use unauthorized AI tools (ChatGPT, Copilot) on sensitive documents to work faster

---

## The Solution

Latent Archon provides a secure, authorized alternative:

### Workspace-Based Document Management
- **Workspaces** isolate documents by program, classification, team, or mission area
- Analysts choose which workspaces to search — cross-workspace queries for broad analysis, or scoped queries for precision
- **Tag-based organization** lets teams categorize documents by type (contract, policy, technical manual, etc.) and filter searches by tag
- Role-based access control ensures need-to-know at the workspace level

### AI-Powered Document Search
- **Semantic search** understands meaning, not just keywords — "What are the maintenance intervals for the APU?" finds the answer even if the document says "auxiliary power unit scheduled servicing"
- **AI-generated responses** synthesize information across multiple documents with full source citations
- Every answer links back to the exact document and passage it was derived from — verifiable, auditable, trustworthy

### Enterprise Integration
- **Microsoft 365 / SharePoint integration** — automatically syncs documents from SharePoint sites and OneDrive via Microsoft Graph API with delta-based incremental updates
- **Workspace mapping** — map SharePoint sites/drives to Latent Archon workspaces for continuous, hands-off document ingestion
- Supports PDF, DOCX, PPTX, XLSX, images (with OCR), and plain text

### Security by Design
- **CUI-compliant** — all data encrypted at rest with customer-managed encryption keys (CMEK) using FIPS 140-2 Level 3 HSM-backed keys
- **DLP scanning** on every uploaded document — automatic detection and redaction of PII, SSN, credit card numbers before indexing
- **Malware scanning** on all uploads before processing
- **Per-tenant data isolation** — row-level security in the database, separate encryption keys per organization
- **Zero training on government data** — AI models are not fine-tuned on uploaded documents; they are used only at query time for retrieval

---

## Security Posture

| Control Area | Implementation |
|-------------|---------------|
| **FedRAMP** | 20x KSI-aligned, OSCAL SSP generated and validated in CI, public compliance repository |
| **Encryption** | AES-256-GCM at rest (CMEK, HSM-backed), TLS 1.3 in transit, per-tenant key isolation |
| **Authentication** | SAML 2.0 / OIDC SSO federation, TOTP MFA enforced on all sessions, session concurrency limiting (AC-10) |
| **Session Management** | Idle timeout (25 min), absolute timeout (12 hr), concurrent session limiting (max 3) |
| **Access Control** | RBAC with org → workspace → document hierarchy, row-level security in database |
| **Data Protection** | DLP scanning on ingest, malware scanning, document-level permissions |
| **Audit** | Immutable audit logs for all data access, exportable to SIEM, Cloud Audit Logs integration |
| **Infrastructure** | Google Cloud Platform with Assured Workloads (IL4/IL5), VPC-isolated, Private Service Connect, no public endpoints except load balancer |
| **Supply Chain** | SBOM generation, dependency scanning (Dependabot + Snyk), container image signing, Artifact Registry with CMEK |
| **Incident Response** | Automated monthly red team exercises (44 attack scenarios), contingency plan testing, FedRAMP-aligned ICP |
| **Continuous Monitoring** | Automated KSI evidence collection, SCN classification on every PR, Cloud Armor WAF |

---

## Infrastructure — Google Cloud Platform

Latent Archon runs entirely on Google Cloud Platform, which holds FedRAMP High authorization and offers Assured Workloads for IL4/IL5 and Dedicated Cloud for IL6.

| Component | GCP Service | Security |
|-----------|------------|----------|
| **Compute** | Cloud Run (serverless containers) | VPC-isolated, no SSH, immutable images |
| **AI/ML** | Gemini (Vertex AI) | Data processing agreement, no model training on inputs |
| **Vector Database** | AlloyDB (PostgreSQL) | CMEK-encrypted, Private Service Connect, automated backups with PITR |
| **Encryption** | Cloud KMS | FIPS 140-2 Level 3 HSM, per-tenant keys, 90-day automatic rotation |
| **Storage** | Cloud Storage | CMEK-encrypted, versioned, regional |
| **Networking** | Cloud Armor + Cloud Load Balancing | WAF, DDoS protection, IP allowlisting |
| **Secrets** | Secret Manager | CMEK-encrypted, IAM-gated, audit-logged |
| **Task Queue** | Cloud Tasks | CMEK-encrypted, async document processing pipeline |
| **Logging** | Cloud Logging + BigQuery | CMEK-encrypted, immutable, SIEM-exportable |
| **Container Registry** | Artifact Registry | CMEK-encrypted, vulnerability scanning, image signing |

### Why GCP?
- **FedRAMP High** authorized — meets the bar for Moderate and High baselines
- **Assured Workloads for IL4/IL5** — data residency, personnel controls, and access transparency built into the platform
- **IL6 via Dedicated Cloud** — path to higher impact levels without re-architecture
- **Vertex AI (Gemini)** — native Google AI with no third-party data processing; covered under Google Cloud's FedRAMP authorization
- **Private Service Connect** — database and internal services never exposed to the public internet

---

## Deployment Tiers

Latent Archon operates three isolated deployment tiers — same codebase, separate infrastructure:

| | **Federal** | **State & Local** | **Commercial** |
|---|---|---|---|
| **Domain** | `fed.latentarchon.com` | `gov.latentarchon.com` | `cloud.latentarchon.com` |
| **Customers** | Federal agencies, DoD, IC | State, local, tribal, territorial | Private sector, enterprises |
| **GCP Environment** | Assured Workloads (IL4/IL5) | Standard GCP + enhanced controls | Standard GCP |
| **FedRAMP** | FedRAMP Moderate | StateRAMP/TX-RAMP if needed | SOC 2 (planned) |
| **Encryption** | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | CMEK, HSM, per-tenant keys | CMEK available |
| **Data Residency** | US-only (Assured Workloads enforced) | US-only (org policy) | Customer's choice |

Each tier runs in its own GCP folder with dedicated projects, databases, encryption keys, VPC networks, and audit logs. No cross-tier data access is possible. See [deployment-tiers.md](deployment-tiers.md) for full architecture details.

### IL4 → IL5 Upgrade Path

A key advantage of GCP over AWS and Azure: upgrading a federal customer from IL4 to IL5 requires **no migration, no downtime, and no new infrastructure**. We enable Assured Workloads IL5 controls on their existing GCP folder — same database, same endpoint, same data.

On AWS, IL5 → IL6 requires migrating to a completely separate Secret Region. On Azure, it requires migrating to Government Secret — a different cloud with different endpoints. On GCP, IL4 → IL5 is a configuration change.

---

## How It Works — A Concrete Example

**Scenario:** An Army PEO contracting officer needs to review 3 years of contract modifications across 12 programs to prepare a briefing on cost growth trends.

1. **Upload** — The contracting team has already uploaded contract folders into program-specific workspaces. New modifications sync automatically from SharePoint overnight via Microsoft Graph integration.

2. **Search** — The officer opens Latent Archon, selects all 12 program workspaces, and asks:
   > *"Summarize all contract modifications that increased cost by more than 10%. Group by program and identify the stated justification for each increase."*

3. **Response** — Latent Archon searches across all documents semantically, finds 47 relevant modifications, and generates a structured summary:
   - Each entry cites the specific modification number, page, and relevant passage
   - Results are grouped by program as requested
   - A synthesis paragraph identifies the three most common justification categories

4. **Verify** — The officer clicks any citation to view the original document passage in context. Every answer is traceable — no hallucination, no unsourced claims.

5. **Refine** — The officer follows up: *"Which of these modifications were sole-source? What was the J&A rationale?"* — Latent Archon narrows the search to the same document set and returns cited answers.

**Time saved:** What previously took 2-3 days of manual document review takes 15 minutes.

---

## Pricing Model

Per-organization subscription based on:
- Number of users
- Document storage volume
- AI query volume

Volume discounts for enterprise-wide and multi-agency deployments. GSA Schedule pricing available (in progress).

All infrastructure costs are included — no separate cloud bills, no hidden AI query fees.

---

## Company

**Latent Archon, Inc.**
- Founded to solve the document intelligence gap in government
- Engineering team with cleared personnel and federal contracting experience
- Contact: gcp-security-admins@latentarchon.com
- Compliance repository: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)

---

*Document ID: CAP-BRIEF-001 | Version: 1.1 | Date: April 2026*
