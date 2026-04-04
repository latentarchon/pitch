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
| **Infrastructure** | Multi-cloud (GCP, AWS, Azure), VPC-isolated, Private Service Connect, no public endpoints except load balancer |
| **Supply Chain** | SBOM generation, dependency scanning (Dependabot + Snyk), container image signing, Artifact Registry with CMEK |
| **Incident Response** | Automated monthly red team exercises (44 attack scenarios), contingency plan testing, FedRAMP-aligned ICP |
| **Continuous Monitoring** | Automated KSI evidence collection, SCN classification on every PR, Cloud Armor WAF |

---

## Multi-Cloud Deployment

Latent Archon deploys on the cloud your agency already uses:

| Cloud | Compute | AI/ML | Vector Store | Encryption |
|-------|---------|-------|-------------|------------|
| **GCP** | Cloud Run | Gemini | AlloyDB | Cloud KMS (HSM) |
| **AWS** | ECS Fargate | Bedrock (Titan/Claude) | Aurora pgvector | KMS |
| **Azure** | Container Apps | Azure OpenAI | Azure PostgreSQL | Key Vault (HSM) |

All deployments use the same codebase, same security controls, same compliance posture.

---

## Deployment Options

- **SaaS (FedRAMP Authorized)** — Latent Archon hosts and operates; agency connects via SSO
- **Dedicated tenant** — Isolated infrastructure in agency's preferred cloud region
- **On-premises / air-gapped** — Container-based deployment for classified or disconnected environments (roadmap)

---

## Pricing Model

Per-organization subscription based on:
- Number of users
- Document storage volume
- AI query volume

Volume discounts for enterprise-wide and multi-agency deployments. GSA Schedule pricing available (in progress).

---

## Company

**Latent Archon, Inc.**
- Founded to solve the document intelligence gap in government
- Engineering team with cleared personnel and federal contracting experience
- Contact: gcp-security-admins@latentarchon.com
- Compliance repository: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)

---

*Document ID: CAP-BRIEF-001 | Version: 1.0 | Date: April 2026*
