# Latent Archon — U.S. Navy & Marine Corps Capability Brief

**Secure Document Intelligence for Naval Forces**

*UNCLASSIFIED — Approved for public distribution*

---

## Mission Relevance

Naval forces operate across globally distributed commands — from NAVAIR and NAVSEA program offices to fleet maintenance activities and Marine Corps systems commands. The sheer volume of technical data packages, acquisition documents, maintenance procedures, and operational orders demands a modern search and analysis capability. Latent Archon provides a CUI-compliant AI platform that enables Navy and Marine Corps personnel to query their documents using natural language, get cited answers, and maintain full chain-of-custody over sensitive data.

---

## Navy & Marine Corps Use Cases

### Naval Aviation (NAVAIR, Fleet Readiness Centers)
- **Aircraft maintenance intelligence** — Semantic search across NAMPs, MIMs, IPBs, and CMMs: *"What is the NDI interval for the F/A-18E wing root fitting?"* — finds the answer across multiple maintenance publications
- **Engineering investigation support** — Upload EIs, TDs, and FCFs into program-specific workspaces; query across all open investigations for a platform
- **Airworthiness document search** — Cross-reference SAR data, flight clearances, and NATOPS changes in seconds
- **CDRL management** — Organize contractor deliverables by workspace per contract; search across deliverables for compliance verification

### Ship systems (NAVSEA, RMCs, Shipyards)
- **Technical manual lookup** — Search across ship-class technical manuals, PMS cards, and MRCs: *"What are the valve lineup procedures for the DDG-51 firemain system?"*
- **Availability planning** — Aggregate and search work package documents, AWRs, and departure reports across maintenance availabilities
- **Class-wide deficiency search** — Query CASREP history and 2-Kilos across an entire ship class to identify systemic material issues

### Marine Corps Systems (MARCORSYSCOM, MCICOM)
- **Expeditionary logistics** — Search across TAMCN documentation, allowance lists, and equipment readiness reports
- **Ground equipment maintenance** — Semantic search across TM-series publications for USMC ground equipment: *"What is the engine oil capacity for the JLTV A1?"*
- **Force design document analysis** — Cross-reference Force Design 2030 documents, concept papers, and capability gap assessments

### Acquisition & Contracting (NAVSUP, FLC)
- **Contract file intelligence** — Upload entire contract folders and query across solicitations, proposals, mods, and CLINs
- **DFARS compliance checking** — Instantly query regulatory requirements: *"What cybersecurity clauses apply to a CUI system contract?"*
- **Source selection support** — Isolate offeror evaluation documents in separate workspaces while enabling cross-reference queries

### Intelligence & Operations (NIWC, Fleet Commands)
- **OPORD/FRAGORD search** — Organize operational orders by exercise or deployment; search across planning documents
- **Lessons learned** — Ingest NLLP products and post-deployment AARs; query across deployments and warfare areas
- **SIGINT/OSINT document triage** — Tag and organize CUI-level analytical products by region, topic, or mission area

---

## Why Latent Archon for the Navy/USMC

| Naval Need | Latent Archon Capability |
|-----------|------------------------|
| CUI/FOUO protection | CMEK encryption, DLP scanning, FedRAMP-aligned controls |
| Navy/MC SSO (Flank Speed) | SAML 2.0 federation compatible with Flank Speed / M365 |
| SharePoint sync | Microsoft Graph integration — auto-sync from Flank Speed SharePoint |
| Ship/squadron/unit isolation | Workspace-level access control — each unit or program gets its own sandbox |
| Cross-platform search | Query across ship classes, aircraft platforms, or programs with tag-based filtering |
| Audit trail | Immutable logs, every AI response traceable to source document and page |
| Cloud platform | Google Cloud Platform with Assured Workloads (IL4/IL5); GCP holds FedRAMP High authorization |

---

## Integration with Navy IT Environment

- **Flank Speed (Microsoft 365)** — SharePoint document sync via Microsoft Graph API with delta-based incremental updates, SAML SSO
- **Navy CAC authentication** — SAML 2.0 federation with Navy ICAM / NMCI identity providers
- **Google Cloud Platform** — FedRAMP High authorized; Assured Workloads for IL4/IL5 with data residency and personnel controls
- **IL4 → IL5 with zero migration** — Unlike AWS GovCloud or Azure Government, upgrading from IL4 to IL5 on GCP is a configuration change, not a re-deployment. Same endpoint, same data, no downtime.
- **DISA STIG compliance** — Containerized deployment follows DoD container hardening guidelines
- **NIST 800-171 / CMMC alignment** — CUI controls mapped to CMMC Level 2 requirements

Federal customers access Latent Archon at `fed.latentarchon.com` — a dedicated, isolated environment separate from state/local and commercial tiers. See [deployment-tiers.md](deployment-tiers.md) for architecture details.

---

## Scenario: Fleet Readiness Center Aircraft Maintenance

**Situation:** An aviation mechanic at FRC East needs to determine the NDI interval for a specific structural component on the F/A-18E. The answer is buried across multiple NAMPs, MIMs, and SRMs — thousands of pages of maintenance publications that use different terminology for the same inspection.

**Before Latent Archon:** The mechanic searches PDF keywords for 20+ minutes, cross-references 3 different publications, and may still miss the most current NAVAIR technical directive that updated the interval.

**With Latent Archon:**
1. Opens the F/A-18E maintenance workspace (pre-loaded with all NAMPs, MIMs, SRMs, and active TDs)
2. Asks: *“What is the current NDI interval for the wing root fitting on the F/A-18E? Include any recent technical directives that modified the original interval.”*
3. Gets a cited answer in seconds — including the original SRM reference AND the recent TD that changed it
4. Clicks citations to verify against the original source documents

**Result:** 20+ minutes of cross-referencing → instant answer with full traceability. Fewer maintenance errors from missed publications.

---

## Getting Started

1. **Pilot program** — 30-day evaluation with a single PMA, SYSCOM directorate, or ship class team
2. **Workspace design** — Map workspaces to your organizational structure (by program, platform, or unit)
3. **Document ingest** — Bulk upload or Flank Speed SharePoint sync
4. **User onboarding** — SSO integration + 1-hour analyst training
5. **Scale** — Expand across programs, type commanders, or fleet activities

---

## Contact

**Latent Archon, Inc.**
- Email: gcp-security-admins@latentarchon.com
- Compliance & security documentation: [github.com/latentarchon/compliance](https://github.com/latentarchon/compliance)
- CAGE Code: *(pending)*
- SAM.gov UEI: *(pending)*

---

*Document ID: CAP-NAVY-001 | Version: 1.1 | Date: April 2026*
