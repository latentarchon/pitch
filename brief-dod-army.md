# Latent Archon — U.S. Army Capability Brief

**Secure Document Intelligence for Army Programs**

*UNCLASSIFIED — Approved for public distribution*

---

## Mission Relevance

The U.S. Army generates and consumes an enormous volume of documents across acquisition, logistics, training, and operations — from Technical Manuals (TMs) and Field Manuals (FMs) to contract files, after-action reviews, and policy memoranda. Latent Archon provides Army organizations with a CUI-compliant AI platform to search, analyze, and extract intelligence from these documents without exposing sensitive data to unauthorized systems.

---

## Army Use Cases

### Acquisition & Contracting (ACC, PEOs)
- **Contract file intelligence** — Upload contract folders (solicitations, proposals, modifications, CDRLs) into a workspace. Ask: *"What are the key performance parameters for this system?"* or *"Summarize all contract modifications related to schedule delays."*
- **Past performance research** — Search across past performance databases and CPARS narratives to identify contractor risk patterns
- **FAR/DFARS compliance** — Instantly query regulatory clauses across the full FAR/DFARS corpus: *"What are the cybersecurity requirements for CUI in DFARS 252.204-7012?"*
- **Source selection support** — Organize evaluation documents by workspace per offeror; search within or across offeror workspaces during evaluation

### Logistics & Sustainment (AMC, ASC, DLA)
- **Technical manual search** — Semantic search across TMs, ETMs, and IETMs: *"What is the torque specification for the M1A2 track tension adjuster?"* — finds the answer even when phrased differently in the manual
- **Supply chain document analysis** — Query shipping documents, inspection reports, and maintenance logs across units
- **Maintenance procedure lookup** — Mechanics and maintainers ask natural-language questions instead of paging through 500-page TMs

### Program Management (PEO Soldier, PEO Aviation, PEO GCS, etc.)
- **Cross-program document search** — Search across multiple program workspaces for lessons learned, test reports, and engineering change proposals
- **Requirements traceability** — Upload CDDs, CPDs, and ICDs into tagged workspaces; query requirements across documents
- **Milestone decision support** — Compile and search supporting documentation for DAB/ASARC reviews

### Training & Doctrine (TRADOC, CAC)
- **Doctrine search** — Semantic search across ADPs, ADRPs, FMs, and ATPs: *"What does FM 3-0 say about multi-domain operations in contested environments?"*
- **Lessons learned** — Ingest CALL (Center for Army Lessons Learned) products and CTC rotation AARs; query across exercises and deployments
- **Course development** — Search existing POIs and curriculum documents to avoid duplication and identify gaps

### Legal (JAG, OSJA)
- **Legal research** — Upload UCMJ references, case files, AR 15-6 investigations, and legal opinions into secure workspaces
- **Policy compliance** — Cross-reference new policies against existing Army Regulations: *"Does this new policy conflict with AR 600-20?"*

---

## Why Latent Archon for the Army

| Army Need | Latent Archon Capability |
|-----------|------------------------|
| CUI protection (DFARS 252.204-7012) | CMEK encryption, DLP scanning, FedRAMP-aligned security controls |
| CAC/SSO integration | SAML 2.0 federation compatible with Army 365 / ICAM |
| SharePoint document sync | Microsoft Graph integration — auto-sync from Army 365 SharePoint sites |
| Need-to-know isolation | Three-layer isolation: database tenant isolation → PostgreSQL RLS → workspace RBAC with joinable workspaces |
| Multi-domain search | Cross-workspace queries across programs while respecting access boundaries |
| Auditability | Immutable audit logs, audit data available for agency SIEM export, every AI response traceable to source |
| Cloud platform | Google Cloud Platform in us-east4 (Northern Virginia) — IL5-aligned, deployed on GCP's FedRAMP High / IL5-authorized infrastructure with Assured Workloads |

---

## Integration with Army IT Environment

- **Army 365 (Microsoft 365)** — SharePoint/OneDrive document sync via Microsoft Graph API with delta-based incremental updates
- **cArmy / Army SSO** — SAML 2.0 federation for single sign-on
- **Google Cloud Platform** — deployed on GCP's FedRAMP High / IL5-authorized infrastructure; Assured Workloads already configured with IL5 compliance regime, US-only data residency, and US-person-only personnel controls; us-east4 (Northern Virginia)
- **IL4 → IL5 with zero migration** — Unlike AWS GovCloud or Azure Government, upgrading from IL4 to IL5 on GCP is a configuration change, not a re-deployment. Same endpoint, same data, no downtime.
- **DISA STIG compliance** — Container-based deployment follows DISA container hardening guidelines
- **NIST 800-171 / CMMC alignment** — CUI controls mapped to CMMC Level 2 requirements

Federal customers access Latent Archon at `fed.latentarchon.com` — a dedicated, isolated GCP environment with Assured Workloads, separate from planned SLTT and commercial tiers. See [deployment-tiers.md](deployment-tiers.md) for architecture details.

---

## Scenario: Logistics Command Technical Manual Search

**Situation:** A maintenance NCO at an AMC depot needs to find the torque specification for a specific component across multiple equipment TMs. The answer exists somewhere in 12,000+ pages of technical manuals for the vehicle family.

**Before Latent Archon:** The NCO spends 45 minutes paging through PDFs, searching for keywords that may not match the document's terminology. Sometimes they give up and call the OEM tech rep.

**With Latent Archon:**
1. Opens the vehicle family workspace (pre-loaded with all TMs for the platform)
2. Asks: *“What is the torque specification for the M1A2 SEPv3 track tension adjuster bolt?”*
3. Gets the answer in 3 seconds with a citation to the exact TM, page, and paragraph
4. Clicks the citation to view the original document page in context

**Result:** 45 minutes → 30 seconds. Multiply across thousands of maintainers.

---

## Getting Started

1. **Pilot program** — 30-day evaluation with a single program office or directorate
2. **Workspace setup** — We configure workspaces mapped to your organizational structure
3. **Document ingest** — Bulk upload or SharePoint sync from existing document repositories
4. **User onboarding** — SSO integration + 1-hour analyst training session
5. **Scale** — Expand to additional programs, directorates, or commands

---

## Contact

**Latent Archon, Inc.**
- Website & demo: [latentarchon.com](https://latentarchon.com)
- Email: contact@latentarchon.com
- Compliance & security documentation: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)
- Capability briefs: [github.com/latentarchon/pitch](https://github.com/latentarchon/pitch)
- CAGE Code: *(pending)*
- SAM.gov UEI: *(pending)*

---

*Document ID: CAP-ARMY-001 | Version: 1.2 | Date: April 2026*
