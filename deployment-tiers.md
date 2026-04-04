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

| | **Federal** | **State & Local (SLTT)** | **Commercial** |
|---|---|---|---|
| **Domain** | `fed.latentarchon.com` | `gov.latentarchon.com` | `cloud.latentarchon.com` |
| **Status** | **Active** — staging deployed | Planned | Planned |
| **Customers** | Federal agencies, DoD, IC | State, local, tribal, territorial | Private sector, enterprises, nonprofits |
| **GCP Environment** | Assured Workloads (IL4/IL5) | Assured Workloads (IL4/IL5) | Standard GCP with org policies |
| **Compliance Certification** | FedRAMP Moderate (in progress) | StateRAMP (planned) | SOC 2 Type II (planned) |
| **Impact Level** | IL4 / IL5 | IL4 / IL5 (exceeds SLTT requirements) | N/A |
| **GCP Region** | us-east4 (Northern Virginia) | us-east4 (Northern Virginia) | us-east4 (Northern Virginia) |
| **Encryption** | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | CMEK, FIPS 140-2 L3 HSM, per-tenant keys | CMEK, FIPS 140-2 L3 HSM, per-tenant keys |
| **Data Residency** | US-only (Assured Workloads enforced) | US-only (Assured Workloads enforced) | US-only (us-east4) |
| **Personnel Controls** | US-person-only access (Assured Workloads) | US-person-only access (Assured Workloads) | Standard Google support |
| **DLP Scanning** | Enforced on all documents | Enforced on all documents | Enforced on all documents |
| **MFA** | Enforced (TOTP) | Enforced (TOTP) | Enforced (TOTP) |
| **Audit Logs** | Immutable, SIEM-exportable | Immutable, SIEM-exportable | Immutable, SIEM-exportable |
| **SSO** | SAML 2.0 / OIDC (required) | SAML 2.0 / OIDC (supported) | SAML 2.0 / OIDC (supported) |

**Why the same security across tiers?** Government-grade security is a competitive advantage, not overhead. State governments and commercial customers get CMEK encryption, DLP scanning, and immutable audit logging by default — not as an upsell. The federal and SLTT tiers both run on Assured Workloads, giving state and local customers IL5-level controls that far exceed typical SLTT requirements.

---

## Infrastructure Isolation

Each tier runs in its own GCP folder with dedicated projects. The federal tier is deployed today; SLTT and commercial folders follow the same pattern.

```
latentarchon.com (GCP Organization)
├── Federal (GCP Folder — Assured Workloads IL5)
│   ├── archon-fed-ops          — Cloud Run, Cloud SQL (PostgreSQL), Cloud Tasks
│   ├── archon-fed-admin        — Admin panel (frontend + backend)
│   ├── archon-fed-app          — User-facing app (frontend + backend)
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
```

### What's Shared (Across All Tiers)
- **Codebase** — Single monorepo, same Go backend, same React frontend
- **Container images** — Built once in CI, promoted across tiers
- **Database schema** — Same migrations, same SQLC queries
- **Security controls** — CMEK encryption, DLP scanning, malware scanning, audit logging, RBAC, row-level security
- **Region** — All tiers deploy to us-east4 (Northern Virginia)

### What's Isolated (Per Tier)
- **GCP projects** — Separate billing, IAM, networking, org policies
- **Databases** — Separate Cloud SQL instances, separate encryption keys
- **Encryption keys** — Separate Cloud KMS keyrings, no cross-tier key access
- **DNS / TLS** — Separate domains, separate certificates
- **Audit logs** — Separate Cloud Logging sinks, no cross-tier log access
- **Identity** — Separate Firebase Auth / Identity Platform projects
- **VPC networks** — No peering between tiers

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
| **2** | Commercial (`cloud.latentarchon.com`) | Planned — same codebase, same security baseline | Post-FedRAMP authorization |
| **3** | SLTT (`gov.latentarchon.com`) | Planned — driven by customer demand | On demand |

The federal tier is the priority because:
- FedRAMP authorization is the hardest credential to earn and the most valuable moat
- Federal contracts are larger and longer-term
- Commercial and SLTT tiers are trivial to stand up once the federal tier is running — same code, same security, new GCP folder

---

## Pricing by Tier

| Tier | Pricing Model | Notes |
|------|--------------|-------|
| **Federal** | Per-org subscription (users + storage + queries) | GSA Schedule pricing (in progress); includes all infra costs |
| **SLTT** | Per-org subscription | Volume discounts for statewide deployments |
| **Commercial** | Per-org subscription | Self-service onboarding (planned) |

---

*Document ID: DEPLOY-TIERS-001 | Version: 1.1 | Date: April 2026*
