# Latent Archon — Government Capability Brief

**Secure Document Intelligence for Federal Agencies**

*UNCLASSIFIED — Approved for public distribution*

---

## Executive Summary

Latent Archon is a FedRAMP High aligned, IL5-aligned, CUI-compliant document intelligence platform that enables federal agencies to upload, organize, and query sensitive documents using AI-powered semantic search and retrieval-augmented generation (RAG). Analysts ask questions in natural language and receive sourced, cited answers drawn exclusively from their agency's own documents — no data leaves the authorization boundary, no training on government data, and every response is traceable to its source material.

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
- **Three-layer data isolation** — (1) database-level tenant isolation, (2) PostgreSQL row-level security (RLS) enforcing tenant boundaries at the query layer, (3) workspace-level application isolation with configurable membership and roles. Even if the application layer has a defect, RLS prevents cross-tenant data access at the database.
- **Joinable workspaces with need-to-know** — users can be members of multiple workspaces with different roles per workspace. Program managers control membership via admin invite or SCIM provisioning from their IdP. Cross-workspace queries are scoped to only the workspaces a user has been granted access to.
- **Zero training on government data** — AI models are not fine-tuned on uploaded documents; they are used only at query time for retrieval

---

## Security Posture

| Control Area | Implementation |
|-------------|---------------|
| **FedRAMP** | 20x KSI-aligned, OSCAL SSP generated and validated in CI, public compliance repository |
| **Encryption** | AES-256-GCM at rest (CMEK, HSM-backed), TLS 1.3 in transit, per-tenant key isolation |
| **Authentication** | SAML 2.0 SSO (federal: MFA enforced by agency IdP, e.g., CAC/PIV; non-federal: app TOTP MFA), session concurrency limiting (AC-10) |
| **Session Management** | Idle timeout (25 min), absolute timeout (12 hr), concurrent session limiting (max 3) |
| **Access Control** | Three-layer isolation: database tenant isolation → PostgreSQL RLS → workspace RBAC. Joinable workspaces with per-workspace roles and need-to-know enforcement |
| **Data Protection** | DLP scanning on ingest, malware scanning, document-level permissions |
| **Audit** | Immutable audit logs for all data access, available for export to agency SIEM upon request, Cloud Audit Logs integration |
| **Infrastructure** | Google Cloud Platform with Assured Workloads (IL4/IL5), VPC-isolated, Private Service Connect, no public endpoints except load balancer |
| **Supply Chain** | SBOM generation, dependency scanning (Dependabot + Snyk), container image signing, Artifact Registry with CMEK |
| **Incident Response** | Automated monthly red team exercises (44 attack scenarios), contingency plan testing, FedRAMP-aligned ICP |
| **Continuous Monitoring** | Automated KSI evidence collection, SCN classification on every PR, Cloud Armor WAF |

---

## Infrastructure — Google Cloud Platform

Latent Archon runs entirely on Google Cloud Platform in **us-east4 (Northern Virginia)**. GCP holds FedRAMP High authorization and offers Assured Workloads for IL4/IL5.

| Component | GCP Service | Security |
|-----------|------------|----------|
| **Compute** | Cloud Run (serverless containers) | VPC-isolated, no SSH, immutable images |
| **AI/ML** | Gemini (Vertex AI) | Data processing agreement, no model training on inputs |
| **Database** | Cloud SQL (PostgreSQL) | CMEK-encrypted, Private Service Connect, automated backups with PITR |
| **Encryption** | Cloud KMS | FIPS 140-2 Level 3 HSM, per-tenant keys, 90-day automatic rotation |
| **Storage** | Cloud Storage | CMEK-encrypted, versioned, regional |
| **Networking** | Cloud Armor + Cloud Load Balancing | WAF, DDoS protection, IP allowlisting |
| **Secrets** | Secret Manager | CMEK-encrypted, IAM-gated, audit-logged |
| **Task Queue** | Cloud Tasks | CMEK-encrypted, async document processing pipeline |
| **Logging** | Cloud Logging + BigQuery | CMEK-encrypted, immutable, available for agency SIEM export |
| **Container Registry** | Artifact Registry | CMEK-encrypted, vulnerability scanning, image signing |

### Why GCP?
- **FedRAMP High aligned and IL5-aligned** — deployed on Google Cloud's FedRAMP High / IL5-authorized platform
- **Assured Workloads deployed today** — Latent Archon's federal tier already runs under Assured Workloads with IL5 compliance regime, US-only data residency, and personnel access controls enforced by Google
- **IL4 → IL5 is a configuration change, not a migration** — no re-deployment, no downtime, no new infrastructure. This is a concrete advantage over platforms that require migration to a separate GovCloud environment
- **Vertex AI (Gemini)** — native Google AI with no third-party data processing; covered under Google Cloud's FedRAMP authorization
- **Private Service Connect** — database and internal services never exposed to the public internet
- **VPC Service Controls** — API-level perimeter preventing data exfiltration even by authorized service accounts acting outside the perimeter

---

## Deployment Tiers (Roadmap)

Latent Archon's architecture defines three isolated deployment tiers — same codebase, same security baseline, separate infrastructure. Only the federal tier is active today; SLTT and commercial tiers are planned.

All tiers run in us-east4 (Northern Virginia) with CMEK encryption (FIPS 140-2 L3 HSM), DLP scanning, MFA, and immutable audit logs.

| | **Federal** | **SLTT** | **Commercial** | **Air-Gapped** |
|---|---|---|---|---|
| **Domain** | `fed.latentarchon.com` | `gov.latentarchon.com` | `cloud.latentarchon.com` | Customer-controlled |
| **Status** | **Active** (staging deployed) | Planned | Planned | **Prototype** |
| **Customers** | Federal agencies, DoD, IC | State, local, tribal, territorial | Private sector, enterprises | DoD/IC classified programs |
| **Platform** | GCP Assured Workloads (IL4/IL5) | GCP Assured Workloads | GCP with org policies | Disconnected infrastructure |
| **Certification** | FedRAMP Moderate (in progress) | StateRAMP (planned) | SOC 2 (planned) | DISA PA (planned) |
| **Encryption** | CMEK, HSM, per-tenant keys | CMEK, HSM, per-tenant keys | CMEK, HSM, per-tenant keys | Platform-managed, FIPS 140-2 |

Each tier runs in isolated infrastructure with dedicated databases, encryption keys, networks, and audit logs. No cross-tier data access is possible. The air-gapped prototype deploys the same codebase via Helm charts to disconnected infrastructure with CAC/PIV mTLS authentication. See [deployment-tiers.md](deployment-tiers.md) for full architecture details.

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

4. **Verify** — The officer clicks any citation to view the original document passage in context. Every answer is traceable to its source — analysts verify before acting.

5. **Refine** — The officer follows up: *"Which of these modifications were sole-source? What was the J&A rationale?"* — Latent Archon narrows the search to the same document set and returns cited answers.

**Time saved:** What previously took 2-3 days of manual document review takes 15 minutes.

---

## How We Compare

| | **Latent Archon** | **Palantir AIP** | **Microsoft Copilot for Gov** | **Custom Bedrock/Azure OpenAI** |
|---|---|---|---|---|
| **Deployment** | Self-service SaaS, 30-day pilot | Multi-million dollar integration, Palantir engineers required | Tied to M365 tenant | Requires cleared dev team to build |
| **Data Isolation** | Three-layer: database tenant isolation → PostgreSQL RLS → workspace RBAC | Platform-managed, opaque to customer | Shared M365 tenant architecture | Whatever you build |
| **Need-to-Know** | Joinable workspaces with per-workspace roles, admin-controlled membership, SCIM provisioning | Configurable but requires Palantir professional services | SharePoint permissions (coarse-grained) | Whatever you build |
| **Source Citations** | Every response cites exact document and passage; analyst verifies before acting | Varies by configuration | Limited citation granularity | Whatever you build |
| **Document Types** | Structured namespace — contracts, TMs, policy, intel products — with tag-based filtering | Generic data ontology | M365 file types only | Whatever you build |
| **CUI Compliance** | FedRAMP High aligned, IL5-aligned, CMEK, DLP, malware scan, immutable audit, Assured Workloads deployed | FedRAMP authorized | FedRAMP authorized (M365) | Depends on implementation |
| **Cost** | Per-org subscription, all infrastructure included | $5M–$50M+ multi-year contracts | Per-user M365 licensing + Copilot add-on | Cloud costs + dev team salaries |
| **Time to Value** | Days (upload docs, start querying) | Months (integration, ontology design, training) | Weeks (if already on M365) | Months (build + ATO) |

**Key differentiator:** Latent Archon is purpose-built for document intelligence with defense-in-depth data isolation. Competing approaches are either general-purpose AI platforms adapted for documents (Palantir, Copilot) or build-it-yourself toolkits (Bedrock, Azure OpenAI) that require significant engineering investment and separate ATO processes.

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
- Website & demo: [latentarchon.com](https://latentarchon.com)
- Contact: contact@latentarchon.com
- Compliance repository: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)
- Capability briefs: [github.com/latentarchon/pitch](https://github.com/latentarchon/pitch)

---

*Document ID: CAP-BRIEF-001 | Version: 1.2 | Date: April 2026*
