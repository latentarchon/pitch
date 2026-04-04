# Latent Archon — U.S. Air Force & Space Force Capability Brief

**Secure Document Intelligence for Air & Space Operations**

*UNCLASSIFIED — Approved for public distribution*

---

## Mission Relevance

The Air Force and Space Force manage vast repositories of technical orders, acquisition documents, operational publications, and engineering data across globally distributed bases and commands. From AFLCMC program offices managing billions in weapon system sustainment to Space Force units standing up new mission areas with rapidly evolving doctrine, the ability to quickly find, synthesize, and analyze information across documents is mission-critical. Latent Archon provides a CUI-compliant AI platform that lets Airmen and Guardians query their documents in natural language and get cited, source-traceable answers.

---

## Air Force Use Cases

### Acquisition & Sustainment (AFLCMC, AFSC)
- **Technical order intelligence** — Semantic search across TOs, TCTOs, and engineering data: *"What is the Phase II inspection interval for the F-16 Block 50 fuel system?"* — finds the answer across multiple TOs even when phrased differently
- **Contract file management** — Upload full contract folders per program; query across solicitations, mods, CDRLs, and CLINs: *"List all contract modifications related to software sustainment for this ACAT II program"*
- **Source selection efficiency** — Workspace-per-offeror isolation during competitive evaluations with cross-workspace analysis capability
- **Diminishing manufacturing sources** — Search across DMSMS case files, bill of materials data, and obsolescence reports to identify supply chain risks

### Flight Operations (MAJCOMs, Wings)
- **AFI/DAFI compliance** — Instantly query regulatory requirements: *"What are the crew rest requirements for C-17 crews under DAFI 11-2C-17 Volume 3?"*
- **Mishap investigation support** — Upload SIBs, maintenance records, and aircrew statements into investigation workspaces; cross-reference across incidents
- **Mission planning document search** — Organize OPLANs, CONOPs, and ATOs by exercise or deployment; search across operational documents

### Maintenance & Logistics (AFMC, LRS, MXGs)
- **TO lookup for maintainers** — Flightline mechanics ask questions instead of paging through Technical Orders: *"What is the torque value for the B-1B inlet cowl fasteners?"*
- **Supply chain analysis** — Search across DIFM, MICAP, and supply documents to identify recurring parts shortages
- **Depot maintenance intelligence** — Query across workload agreements, repair procedures, and quality deficiency reports at ALC level

### Test & Evaluation (AFTC, Edwards, Eglin)
- **Test report search** — Semantic search across OT&E and DT&E reports, test plans, and flight test data: *"What were the radar performance findings from the Block 4 OUE at Nellis?"*
- **Cross-program lessons learned** — Query test results across weapon systems to identify common integration issues

## Space Force Use Cases

### Space Acquisition (SpRCO, SSC)
- **Rapid acquisition documentation** — Space Force's accelerated acquisition timelines demand fast document analysis; search across MEAs, BCLs, and ORDs for new space programs
- **Satellite system documentation** — Organize technical documentation by constellation or mission area; query across programs for design heritage and lessons learned

### Space Operations (SpOC, Delta units)
- **Procedure and checklist search** — Query across satellite operations procedures, anomaly resolution guides, and contingency checklists
- **Space domain awareness** — Organize CUI-level analytical products by orbital regime, threat type, or mission area

---

## Why Latent Archon for USAF/USSF

| Air/Space Force Need | Latent Archon Capability |
|---------------------|------------------------|
| CUI protection (DFARS 252.204-7012) | CMEK encryption, DLP scanning, FedRAMP-aligned controls |
| AF/SF SSO integration | SAML 2.0 federation compatible with AF365 / Cloud One |
| SharePoint sync | Microsoft Graph integration — auto-sync from AF365 SharePoint sites |
| Program/unit isolation | Workspace-level access control — each SPO, squadron, or wing gets its own sandbox |
| Cross-system search | Query across weapon systems, programs, or commands with tag-based filtering |
| Auditability | Immutable logs, every AI response traceable to source document |
| Cloud platform | Google Cloud Platform in us-east4 (Northern Virginia) with Assured Workloads (IL4/IL5); GCP holds FedRAMP High authorization |

---

## Integration with AF/SF IT Environment

- **AF365 (Microsoft 365)** — SharePoint document sync via Microsoft Graph API with delta-based incremental updates, SAML SSO
- **CAC authentication** — SAML 2.0 federation with Air Force ICAM
- **Google Cloud Platform** — FedRAMP High authorized; Assured Workloads for IL4/IL5 with data residency and US-person-only personnel controls; us-east4 (Northern Virginia)
- **IL4 → IL5 with zero migration** — Unlike AWS GovCloud or Azure Government, upgrading from IL4 to IL5 on GCP is a configuration change, not a re-deployment. Same endpoint, same data, no downtime.
- **Platform One / Cloud One** — Container-based architecture compatible with AF PaaS environments
- **DISA STIG compliance** — Follows DoD container hardening guidelines and NIST 800-171

Federal customers access Latent Archon at `fed.latentarchon.com` — a dedicated, isolated GCP environment with Assured Workloads, separate from planned SLTT and commercial tiers. See [deployment-tiers.md](deployment-tiers.md) for architecture details.

---

## Scenario: Weapon System Sustainment Program Office

**Situation:** A program manager at AFLCMC needs to prepare for an ACAT II milestone review. The supporting documentation spans 8 years of contract files, test reports, engineering change proposals, and CDRLs across 3 prime contractors — tens of thousands of pages.

**Before Latent Archon:** A team of 4 analysts spends 2 weeks manually pulling documents from SharePoint, reading through files, and compiling a briefing package. Key documents are missed because they're filed under a different contract or program name.

**With Latent Archon:**
1. All program documents are already in workspaces organized by contractor and document type, auto-synced from AF365 SharePoint
2. The PM asks: *“Summarize all test failures and corrective actions from OT&E reports across all three contractors for this system. Include the current status of each corrective action.”*
3. Latent Archon searches across all three contractor workspaces, finds 23 relevant test findings, and generates a structured summary with citations to each OT&E report
4. Follow-up: *“Which of these corrective actions are still open? Cross-reference with the latest CDRLs to confirm.”*

**Result:** 2 weeks of manual compilation → 1 hour of AI-assisted analysis. Better coverage, fewer missed documents.

---

## Getting Started

1. **Pilot program** — 30-day evaluation with a single SPO, test squadron, or directorate
2. **Workspace design** — Map workspaces to organizational structure (by program, MDS, or unit)
3. **Document ingest** — Bulk upload or AF365 SharePoint sync
4. **User onboarding** — SSO integration + 1-hour analyst training
5. **Scale** — Expand across programs, wings, or MAJCOMs

---

## Contact

**Latent Archon, Inc.**
- Email: gcp-security-admins@latentarchon.com
- Compliance & security documentation: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)
- CAGE Code: *(pending)*
- SAM.gov UEI: *(pending)*

---

*Document ID: CAP-USAF-001 | Version: 1.1 | Date: April 2026*
