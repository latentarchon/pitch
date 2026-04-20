# Latent Archon — Multi-Tier Deployment Architecture (Roadmap)

**Secure Isolation Across Customer Segments**

*UNCLASSIFIED — Approved for public distribution*

---

## Overview

Latent Archon's long-term architecture defines three deployment tiers, each running the same codebase and container images but isolated at the infrastructure level with distinct compliance certifications, GCP organizational policies, and domain endpoints.

**Current status:** Only the federal tier (`fed.latentarchon.com`) is active, with staging deployed and FedRAMP authorization in progress. The state/local and commercial tiers are planned roadmap items — the infrastructure is identical and can be stood up quickly once there is customer demand. We are not presently certified at FedRAMP, StateRAMP, or SOC 2.

---

## Tier Architecture (Roadmap)

All tiers run on Google Cloud Platform in **us-east4 (Northern Virginia)** with the same high-security baseline: CMEK encryption with FIPS 140-2 Level 3 HSM-backed keys, DLP scanning, MFA enforcement, immutable audit logs, and row-level security. The tiers differ in compliance certification and Assured Workloads controls, not in security posture.

| | **Federal** | **State & Local (SLTT)** | **Commercial** | **Air-Gapped (Prototype)** |
|---|---|---|---|---|
| **Domain** | `fed.latentarchon.com` | `gov.latentarchon.com` | `cloud.latentarchon.com` | Customer-controlled |
| **Status** | **Active** — staging deployed | Planned | Planned | **Prototype** — architecture developed |
| **Customers** | Federal agencies, DoD, IC | State, local, tribal, territorial | Private sector, enterprises, nonprofits | DoD/IC classified programs |
| **Platform** | GCP Assured Workloads (IL4/IL5) | GCP Assured Workloads | GCP with org policies | Disconnected infrastructure |
| **Compliance Certification** | FedRAMP Moderate (in progress) | StateRAMP (planned) | SOC 2 Type II (planned) | DISA PA (planned) |
| **Impact Level** | IL4 / IL5 (DoD designation) | N/A (same Assured Workloads controls) | N/A | IL6 / Secret |
| **Region** | us-east4 (Northern Virginia) | us-east4 (Northern Virginia) | us-east4 (Northern Virginia) | Customer facility |
| **Encryption** | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | Platform-managed, FIPS 140-2 |
| **Data Residency** | US-only (Assured Workloads enforced) | US-only (Assured Workloads enforced) | US-only (us-east4 org policy) | Physical air-gap |
| **Personnel Controls** | US-person-only access (Assured Workloads) | US-person-only access (Assured Workloads) | Standard Google support | Cleared personnel only |
| **DLP Scanning** | Enforced on all documents | Enforced on all documents | Enforced on all documents | Enforced on all documents |
| **Authentication** | SAML 2.0 SSO (required); MFA enforced by agency IdP (e.g., CAC/PIV) | SAML 2.0 / OIDC or app TOTP MFA | SAML 2.0 / OIDC or app TOTP MFA | CAC/PIV mTLS (certificate-based) |
| **Audit Logs** | Immutable, available for agency SIEM export | Immutable, available for agency SIEM export | Immutable, available for SIEM export | Immutable, within air-gapped boundary |

**Why the same security across tiers?** Government-grade security is a competitive advantage, not overhead. State governments and commercial customers get CMEK encryption, DLP scanning, and immutable audit logging by default — not as an upsell. The SLTT tier runs on the same Assured Workloads infrastructure as the federal tier, providing US-only data residency, US-person-only access controls, and access transparency — far exceeding typical StateRAMP requirements. (Note: Impact Levels are a DoD designation and do not apply to SLTT customers, but the underlying controls are identical.)

**Air-gapped tier (prototype):** The same Go backend and React frontend run on disconnected infrastructure with platform-native database, object storage, and AI/ML services — no internet connectivity, CAC/PIV certificate-based authentication via mTLS, and Kubernetes-native deployment via Helm charts. This prototype demonstrates that the Latent Archon codebase can span from commercial cloud to physically isolated classified environments without forking. Contact us for details.

---

## Infrastructure Isolation

Each tier runs in its own GCP folder with dedicated projects. The federal tier is deployed today; SLTT and commercial folders follow the same pattern.

```
latentarchon.com (GCP Organization)
├── Federal (GCP Folder — Assured Workloads)
│   ├── archon-fed-ops          — Backend: Cloud Run services, Cloud SQL (PostgreSQL), Cloud Tasks
│   ├── archon-fed-admin        — Admin panel: frontend + thin API layer
│   ├── archon-fed-app          — User-facing app: frontend + thin API layer
│   └── archon-fed-kms          — Cloud KMS keys (HSM-backed)
│
├── SLTT (GCP Folder — Assured Workloads) [PLANNED]
│   ├── archon-gov-ops
│   ├── archon-gov-admin
│   ├── archon-gov-app
│   └── archon-gov-kms
│
└── Commercial (GCP Folder) [PLANNED]
    ├── archon-cloud-ops
    ├── archon-cloud-admin
    ├── archon-cloud-app
    └── archon-cloud-kms

Air-Gapped [PROTOTYPE]
├── Kubernetes Deployments (app, admin, ops, worker)
├── Platform-native database (PostgreSQL-compatible, RLS)
├── Platform-native object storage
├── Platform-native AI/ML services
└── mTLS ingress (CAC/PIV)
```

### What's Shared (Across All Tiers)
- **Codebase** — Single monorepo, same Go backend, same React frontend
- **Container images** — Built once in CI, promoted across tiers
- **Database schema** — Same migrations, same SQLC queries
- **Security controls** — CMEK encryption, DLP scanning, malware scanning, audit logging, RBAC, row-level security
- **Region** — All tiers deploy to us-east4 (Northern Virginia)

### What's Isolated (Per Tier)
- **GCP projects** — Separate billing, IAM, org policies
- **VPC networks** — Separate VPCs per tier, no peering between tiers
- **Compute** — Separate Cloud Run services per tier
- **Databases** — Separate Cloud SQL instances, separate encryption keys
- **Task processing** — Separate Cloud Tasks queues and Cloud Scheduler jobs
- **Storage** — Separate Cloud Storage buckets
- **Encryption keys** — Separate Cloud KMS keyrings (HSM-backed), no cross-tier key access
- **Secrets** — Separate Secret Manager instances
- **DNS / TLS** — Separate domains, separate certificates, separate Cloud Load Balancers
- **Audit logs** — Separate Cloud Logging sinks and BigQuery datasets, no cross-tier log access
- **Identity** — Separate Firebase Auth / Identity Platform projects
- **Artifact Registry** — Separate container registries per tier
- **WAF** — Separate Cloud Armor policies

---

## The IL4 → IL5 Upgrade Path

A key advantage of GCP over AWS and Azure for DoD customers:

| Cloud | IL4 → IL5 | What It Means |
|-------|-----------|---------------|
| **AWS** | Same GovCloud region, but additional dedicated tenancy configurations required | Re-configuration, potential service disruption |
| **Azure** | Azure Government → additional isolation configs | Different configuration, potential endpoint changes |
| **GCP** | **Enable Assured Workloads on the same project** | No migration, no downtime, no new infrastructure |

Upgrading from IL4 to IL5 on GCP is a configuration change — same database, same endpoint, same data, zero downtime. This is a concrete selling point for DoD customers who've experienced the pain of cloud environment migrations.

---

## Tier Rollout Plan

| Phase | Tier | Status | Timeline |
|-------|------|--------|----------|
| **1** | Federal (`fed.latentarchon.com`) | Active — staging deployed, FedRAMP in progress | Now |
| **2** | Air-Gapped (classified) | Prototype — deployment architecture developed | Customer-driven |
| **3** | Commercial (`cloud.latentarchon.com`) | Planned — same codebase, same security baseline | Post-FedRAMP authorization |
| **4** | SLTT (`gov.latentarchon.com`) | Planned — driven by customer demand | On demand |

The federal tier is the priority because:
- FedRAMP authorization is the hardest credential to earn and the most valuable moat
- Federal contracts are larger and longer-term
- Commercial and SLTT tiers are trivial to stand up once the federal tier is running — same code, same security, new GCP folder

---

*Document ID: DEPLOY-TIERS-001 | Version: 1.3 | Date: April 2026*
