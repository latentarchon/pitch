# Latent Archon — Multi-Tier Deployment Architecture

**Secure Isolation Across Customer Segments**

*UNCLASSIFIED — Approved for public distribution*

---

## Overview

Latent Archon operates three deployment tiers, each running the same codebase and container images but isolated at the infrastructure level with distinct compliance postures, GCP organizational policies, and domain endpoints. This architecture enables us to serve federal agencies, state and local governments, and commercial customers from a single product while maintaining strict authorization boundary separation.

---

## Tier Architecture

| | **Federal** | **State & Local** | **Commercial** |
|---|---|---|---|
| **Domain** | `fed.latentarchon.com` | `gov.latentarchon.com` | `cloud.latentarchon.com` |
| **Customers** | Federal agencies, DoD, IC | State, local, tribal, territorial (SLTT) | Private sector, enterprises, nonprofits |
| **GCP Environment** | Assured Workloads (IL4/IL5) | Standard GCP with enhanced controls | Standard GCP |
| **FedRAMP** | FedRAMP Moderate authorized | Not required (StateRAMP/TX-RAMP if needed) | Not required |
| **Impact Level** | IL4 / IL5 (IL6 via Dedicated Cloud roadmap) | N/A | N/A |
| **Encryption** | CMEK with FIPS 140-2 Level 3 HSM, per-tenant keys | CMEK with HSM, per-tenant keys | CMEK available, Google-managed default |
| **Data Residency** | US-only (Assured Workloads enforced) | US-only (org policy enforced) | Customer's choice of GCP region |
| **Personnel Controls** | US-person-only access (Assured Workloads) | US-based support | Standard Google support |
| **Compliance Reporting** | FedRAMP ConMon, POA&M, annual assessment | StateRAMP ConMon (if applicable) | SOC 2 Type II (planned) |
| **DLP Scanning** | Enforced on all documents | Enforced on all documents | Enforced on all documents |
| **MFA** | Enforced (TOTP) | Enforced (TOTP) | Enforced (TOTP) |
| **Audit Logs** | Immutable, SIEM-exportable, FedRAMP retention | Immutable, SIEM-exportable | Immutable, exportable |
| **SSO** | SAML 2.0 / OIDC (required) | SAML 2.0 / OIDC (supported) | SAML 2.0 / OIDC (supported) |

---

## Infrastructure Isolation

Each tier runs in its own GCP folder with dedicated:

```
latentarchon.com (GCP Organization)
├── Federal (GCP Folder — Assured Workloads IL5)
│   ├── archon-fed-ops          — Cloud Run, AlloyDB, Cloud Tasks
│   ├── archon-fed-admin        — Admin panel backend + frontend
│   ├── archon-fed-app          — User-facing frontend
│   └── archon-fed-kms          — Cloud KMS keys (HSM-backed)
│
├── State & Local (GCP Folder — Standard + enhanced org policies)
│   ├── archon-gov-ops
│   ├── archon-gov-admin
│   ├── archon-gov-app
│   └── archon-gov-kms
│
└── Commercial (GCP Folder — Standard)
    ├── archon-cloud-ops
    ├── archon-cloud-admin
    ├── archon-cloud-app
    └── archon-cloud-kms
```

### What's Shared (Across All Tiers)
- **Codebase** — Single monorepo, same Go backend, same React frontend
- **Container images** — Built once in CI, promoted across tiers
- **Database schema** — Same migrations, same SQLC queries
- **Security controls** — DLP, malware scanning, audit logging, RBAC, row-level security

### What's Isolated (Per Tier)
- **GCP projects** — Separate billing, IAM, networking, org policies
- **Databases** — Separate AlloyDB clusters, separate encryption keys
- **Encryption keys** — Separate Cloud KMS keyrings, no cross-tier key access
- **DNS / TLS** — Separate domains, separate certificates
- **Audit logs** — Separate Cloud Logging sinks, no cross-tier log access
- **Identity** — Separate Firebase Auth / Identity Platform projects
- **VPC networks** — No peering between tiers

---

## The IL4 → IL5 Upgrade Path

One of the most significant advantages of GCP over AWS and Azure for DoD customers:

### The Problem with Other Clouds

| Cloud | IL4 → IL5 | What It Means |
|-------|-----------|---------------|
| **AWS** | Same GovCloud region, but additional dedicated tenancy configs; IL6 requires migration to Secret Region | Potential re-deployment for IL6; different API endpoints, different service availability |
| **Azure** | Azure Government → Azure Government with isolation configs; IL6 requires Government Secret | Different environment, different endpoints, different Azure AD tenant |
| **GCP** | **Enable Assured Workloads on the same project** | No migration, no downtime, no new infrastructure |

### How It Works on GCP

1. Customer starts on `fed.latentarchon.com` at IL4
2. Agency's authorization boundary expands to require IL5
3. We enable Assured Workloads IL5 controls on their GCP folder
4. GCP enforces: US-only data residency, US-person-only access, Access Transparency logging
5. **Same database, same endpoint, same data, zero downtime**

This is a concrete selling point for DoD customers evaluating competing platforms — especially those who've experienced the pain of migrating between AWS commercial and GovCloud, or between Azure Government and Azure Government Secret.

---

## Tier Rollout Plan

| Phase | Tier | Status | Timeline |
|-------|------|--------|----------|
| **1** | Federal (`fed.latentarchon.com`) | Active — staging deployed, FedRAMP in progress | Now |
| **2** | Commercial (`cloud.latentarchon.com`) | Planned — same codebase, lighter compliance | Post-FedRAMP authorization |
| **3** | State & Local (`gov.latentarchon.com`) | Planned — driven by SLTT customer demand | On demand |

The federal tier is the priority because:
- FedRAMP authorization is the hardest credential to earn and the most valuable moat
- Federal contracts are larger and longer-term
- Commercial and SLTT tiers are trivial to stand up once the federal tier is running — it's the same code with fewer restrictions

---

## Pricing by Tier

| Tier | Pricing Model | Notes |
|------|--------------|-------|
| **Federal** | Per-org subscription (users + storage + queries) | GSA Schedule pricing (in progress); includes all infra costs |
| **State & Local** | Per-org subscription | Volume discounts for statewide deployments |
| **Commercial** | Per-org subscription | Self-service onboarding (planned); lower price point than federal |

---

*Document ID: DEPLOY-TIERS-001 | Version: 1.0 | Date: April 2026*
