# Latent Archon — Law Enforcement Capability Brief

**Secure Document Intelligence for Federal, State, and Local Law Enforcement**

*UNCLASSIFIED — Approved for public distribution*

---

## Mission Relevance

Law enforcement agencies at every level — federal, state, and local — manage vast collections of case files, investigative reports, policies, SOPs, training records, legal guidance, and interagency correspondence. Officers and analysts spend hours searching disconnected systems for information that should be at their fingertips. Latent Archon provides a CJIS-compliant AI platform that lets law enforcement personnel ask questions in plain English and receive cited, source-traceable answers drawn exclusively from their agency's own authorized documents.

---

## Law Enforcement Use Cases

### Case File Intelligence
- **Cross-case pattern search** — Search across case files semantically: *"Find all cases in the last 3 years involving wire fraud schemes targeting elderly victims with losses over $100K"*
- **Witness and evidence linking** — Query across multiple case workspaces to find connections: *"Has this address appeared in any other open investigations?"*
- **Case preparation** — Quickly compile relevant evidence and prior case law for prosecutorial briefings

### Policy and SOP Search
- **Use-of-force policy** — Instant answers from policy documents: *"What are the de-escalation requirements before deploying less-lethal force?"*
- **Pursuit policy** — *"Under what circumstances is a vehicle pursuit authorized and what are the supervisor notification requirements?"*
- **Administrative procedures** — Search across general orders, directives, and SOPs without knowing which document contains the answer

### Training and Standards
- **POST standards compliance** — Search across Peace Officer Standards and Training requirements, academy curricula, and continuing education records
- **Legal updates** — Query across recent court decisions, AG opinions, and legal bulletins that affect operations: *"What are the current search and seizure requirements for vehicle stops after [case name]?"*
- **Field training documentation** — Organize FTO records, daily observation reports, and remediation plans by trainee workspace

### Federal Law Enforcement
- **FBI** — Cross-reference intelligence products, case files, and legal guidance across field offices
- **DEA** — Search across case files, drug scheduling documents, and interagency MOUs
- **ATF** — Query across firearms trace data reports, inspection records, and regulatory guidance
- **USMS** — Search across fugitive case files, prisoner transport records, and operational plans
- **CBP/ICE** — Cross-reference inspection records, enforcement actions, and policy guidance
- **Secret Service** — Search across protective intelligence reports, financial crime case files, and threat assessments

### State and Local Law Enforcement
- **Police departments** — Policy manual search, case file management, training records
- **Sheriff's offices** — Warrant tracking documentation, civil process records, jail operations SOPs
- **State police/highway patrol** — Crash report analysis, policy compliance, training standards
- **Fusion centers** — Cross-agency intelligence product search and analytical synthesis
- **District attorneys** — Case file review, precedent search, plea agreement history

---

## Why Latent Archon for Law Enforcement

| LE Need | Latent Archon Capability |
|---------|------------------------|
| CJIS Security Policy compliance | CJIS v5.9.5 mapped — 10 policy areas MET, 2 INHERITED from GCP, 1 PARTIAL (see compliance docs) |
| CUI/CJI protection | CMEK encryption (FIPS 140-2 Level 3 HSM), automated DLP scanning, per-agency encryption keys |
| Agency SSO / CAC / PIV | SAML 2.0 / OIDC federation with any identity provider |
| Case-level access control | Workspace-level isolation — each case, unit, or program gets its own access-controlled sandbox |
| Cross-case search | Selective cross-workspace queries — investigators choose which case files to include |
| Audit trail | Immutable logs, every AI response traceable to source document, audit data available for agency SIEM export |
| Cloud platform | Google Cloud Platform in us-east4 (Northern Virginia); GCP holds FedRAMP High authorization |
| Personnel security | CJIS Security Addendum signed by all personnel with CJI access; US-person-only access via Assured Workloads |

### CJIS Compliance

Latent Archon maintains a public CJIS compliance mapping against FBI CJIS Security Policy v5.9.5, covering all 13 policy areas. Key controls include:

- **Management Control Agreement (MCA)** template ready for execution with each agency
- **CJIS Security Addendum** signed by all personnel before production access
- **Advanced authentication** — MFA enforced on all sessions (SAML SSO with IdP-enforced MFA or app TOTP)
- **Encryption in transit and at rest** — TLS 1.3, AES-256-GCM with FIPS 140-2 Level 3 HSM-backed keys
- **Personnel security screening** — Background checks, security awareness training within 5 business days of access
- **Audit logging** — Immutable audit trail for all CJI access, with configurable retention

Full CJIS compliance documentation: [github.com/latentarchon/compliance/tree/main/cjis](https://github.com/latentarchon/compliance/tree/main/cjis)

---

## Deployment

Federal law enforcement agencies access Latent Archon at `fed.latentarchon.com` — a dedicated, isolated GCP environment with Assured Workloads. State and local law enforcement will use a separate isolated environment at `gov.latentarchon.com` (planned), running on the same Assured Workloads infrastructure with CJIS-compliant controls. All tiers run the same security baseline: CMEK encryption, DLP scanning, MFA, immutable audit logs. See [deployment-tiers.md](deployment-tiers.md) for architecture details.

---

## Scenario: Cold Case Review

**Situation:** A detective unit is reviewing cold cases for potential new leads. They have 47 unsolved cases spanning 15 years — each with hundreds of pages of investigative reports, witness statements, forensic analyses, and supplemental reports stored across file cabinets and scanned PDFs.

**Before Latent Archon:** A detective assigned to review cold cases reads through each case file manually, taking notes and trying to identify patterns or connections. Reviewing a single case takes 2-3 hours. Finding connections across cases requires institutional knowledge that retired detectives took with them.

**With Latent Archon:**
1. All 47 case files are uploaded into individual case workspaces with access restricted to the cold case unit
2. The detective selects all case workspaces and asks: *"Which cases involve similar physical descriptions of suspects — white male, 30-40, tattoo on left forearm? Include witness statement details and dates."*
3. Latent Archon searches across all case files semantically, identifies 4 cases with matching descriptions from witness statements across different years, and presents them with full citations to the specific statements
4. Follow-up: *"Were any of these cases in the same geographic area? What forensic evidence was collected?"*

**Result:** Weeks of manual cold case review → hours. Connections that require reading thousands of pages are surfaced instantly. A pattern across 4 cases that no single detective could have spotted becomes obvious.

---

## Scenario: Policy Compliance After Use-of-Force Incident

**Situation:** Internal affairs is reviewing a use-of-force incident. They need to determine whether the officer's actions complied with departmental policy, state law, and POST training standards. The relevant documents span the department's general orders, state statute, POST commission guidelines, and the officer's training records.

**Before Latent Archon:** An IA investigator manually searches through the policy manual (400+ pages), state statutes, and POST guidelines separately. They may miss a recently updated directive or training bulletin.

**With Latent Archon:**
1. Workspaces are organized by document type: departmental policies, state statutes, POST standards, training records
2. The investigator asks: *"What are the department's requirements for use of force reporting, including timelines, required forms, and supervisor notification?"*
3. Latent Archon returns the specific general order sections, any recent policy memos that modified the requirements, and the POST training standard — all cited with document and page
4. Follow-up: *"Did this officer complete the most recent use-of-force training update? When was it completed?"*

**Result:** Hours of cross-referencing policies → minutes. Complete, cited answers that ensure no relevant policy or standard is overlooked.

---

## Getting Started

1. **Pilot** — 30-day evaluation scoped to a single unit, division, or case type
2. **CJIS compliance review** — We provide our CJIS compliance mapping and execute an MCA with your agency
3. **Workspace design** — Map workspaces to your organizational structure (by unit, case type, or document category)
4. **Document ingest** — Bulk upload of case files, policies, and training documents
5. **User onboarding** — SSO integration + 1-hour staff training
6. **Scale** — Expand across divisions, precincts, or the entire department

---

## Contact

**Latent Archon, Inc.**
- Website & demo: [latentarchon.com](https://latentarchon.com)
- Email: gcp-security-admins@latentarchon.com
- Compliance & security documentation: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)
- CJIS compliance mapping: [github.com/latentarchon/compliance/tree/main/cjis](https://github.com/latentarchon/compliance/tree/main/cjis)
- Capability briefs: [github.com/latentarchon/pitch](https://github.com/latentarchon/pitch)
- CAGE Code: *(pending)*
- SAM.gov UEI: *(pending)*

---

*Document ID: CAP-LE-001 | Version: 1.0 | Date: April 2026*
