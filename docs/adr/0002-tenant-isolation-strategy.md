# ADR-0002 — Tenant Isolation Strategy

* Status: Proposed
* Date: 2026-05-13
* Decision Owner: Integrity Sentinel Architecture
* Category: SaaS Architecture

---

# Context

Integrity Sentinel is envisioned as a multi-tenant enterprise SaaS platform focused on:

* operational integrity monitoring
* fraud detection
* insider risk analysis
* AI-assisted investigations
* relationship intelligence
* governance workflows

The platform is intended to support multiple enterprise customers (tenants) while ensuring:

* strong data isolation
* secure tenant boundaries
* operational scalability
* efficient platform management
* tenant-aware analytics
* compliance-aligned governance

The architecture must establish a tenancy model capable of supporting:

* enterprise SaaS deployment
* role-based access control
* tenant-specific policies
* tenant-scoped investigations
* tenant-scoped graph intelligence
* AI-assisted operational workflows

---

# Problem Statement

Integrity Sentinel requires a tenant isolation strategy that balances:

* security
* scalability
* operational simplicity
* cost efficiency
* performance isolation
* tenant governance
* future enterprise growth

The architecture must determine:

* how tenant boundaries are enforced,
* how tenant data is partitioned,
* how authentication and authorization are scoped,
* and how operational services maintain tenant awareness.

---

# Architectural Requirements

The tenancy model must support:

## Security Requirements

* strict tenant data isolation
* tenant-scoped authorization
* prevention of cross-tenant data access
* auditable tenant boundaries
* secure identity integration

---

## Operational Requirements

* scalable onboarding
* centralized platform operations
* manageable deployment complexity
* observability across tenants
* tenant-aware workflows

---

## Product Requirements

* tenant-specific policies
* tenant-specific dashboards
* tenant-specific investigations
* tenant-scoped risk analytics
* configurable governance models

---

## AI & Analytics Requirements

* tenant-scoped graph intelligence
* tenant-scoped vector retrieval
* tenant-aware AI investigations
* isolated evidence correlation
* explainable tenant-specific analytics

---

# Evaluated Options

## Option A — Fully Shared Multi-Tenant Database

### Description

All tenants share:

* application services
* databases
* graph stores
* storage accounts

Tenant isolation enforced through:

* tenant identifiers
* application-layer filtering
* RBAC

---

### Advantages

* simplest deployment model
* lowest infrastructure cost
* easiest operational management
* simplified scaling
* centralized observability

---

### Disadvantages

* highest cross-tenant risk
* increased authorization sensitivity
* difficult enterprise assurance
* weaker isolation guarantees
* increased blast radius
* more difficult compliance positioning

---

### Assessment

Suitable for:

* early-stage prototype
* internal proof-of-concept
* low-sensitivity workloads

Not ideal for:

* enterprise trust platform positioning
* highly sensitive investigations
* enterprise compliance expectations

---

# Option B — Shared Services + Tenant-Scoped Data Isolation

### Description

Shared platform services:

* frontend
* APIs
* orchestration
* AI services
* observability

Tenant-scoped data separation:

* logical tenant partitioning
* tenant-aware RBAC
* tenant-scoped graph partitioning
* tenant-scoped vector indexes
* tenant-scoped storage containers

---

### Advantages

* balanced operational model
* scalable SaaS architecture
* reduced infrastructure overhead
* manageable deployment complexity
* stronger tenant isolation
* supports enterprise scalability
* Azure-native alignment

---

### Disadvantages

* increased architectural complexity
* strong authorization discipline required
* tenant-aware services mandatory
* careful observability partitioning required

---

### Assessment

Strong candidate for:

* enterprise SaaS platform
* operational scalability
* managed cloud operations
* medium-to-large tenant environments

Provides strong balance between:

* isolation
* scalability
* operational manageability
* cost efficiency

---

# Option C — Dedicated Infrastructure Per Tenant

### Description

Each tenant receives:

* dedicated databases
* dedicated graph stores
* dedicated storage
* dedicated application instances

Potentially:

* dedicated subscriptions
* dedicated ACA environments
* dedicated networking boundaries

---

### Advantages

* strongest isolation guarantees
* strongest enterprise assurance
* minimal cross-tenant risk
* easier compliance positioning
* simplified tenant-level governance

---

### Disadvantages

* very high operational complexity
* significantly increased infrastructure cost
* difficult operational scaling
* deployment management overhead
* reduced platform efficiency

---

### Assessment

Potentially appropriate for:

* regulated enterprise environments
* government workloads
* highly sensitive tenants

Potentially excessive for:

* early-stage SaaS platform
* standard commercial deployments

---

# Option D — Hybrid Isolation Model

### Description

Support multiple tenancy tiers:

## Standard Tenancy

Shared services with tenant-scoped logical isolation.

## Premium / Regulated Tenancy

Dedicated infrastructure or isolated data environments.

---

### Advantages

* flexible enterprise positioning
* supports premium enterprise offerings
* scalable SaaS baseline
* future-proof commercial strategy

---

### Disadvantages

* increased operational complexity
* more complex deployment orchestration
* more complex tenant lifecycle management

---

### Assessment

Strong long-term enterprise strategy.

Potentially introduced after initial SaaS maturity.

---

# Decision

## Selected Direction

The proposed architecture adopts:

> Shared platform services with tenant-scoped data isolation.

This corresponds to:

> Option B — Shared Services + Tenant-Scoped Data Isolation

---

# Decision Rationale

The selected model provides the strongest balance between:

* operational scalability
* enterprise SaaS efficiency
* tenant isolation
* Azure-native deployment
* platform manageability
* future extensibility

The architecture intentionally separates:

* shared operational services
* tenant-scoped data domains
* tenant-aware authorization
* tenant-aware AI workflows

---

# Proposed Tenant Isolation Model

## Shared Platform Services

Shared services include:

* frontend application
* API gateway
* orchestration services
* AI investigation services
* observability stack
* deployment infrastructure

These services remain:

* tenant-aware
* authorization-scoped
* claims-driven

---

## Tenant-Scoped Operational Data

Azure SQL logical isolation through:

* tenant identifiers
* tenant-aware RBAC
* tenant-scoped query enforcement
* service-layer authorization

Potential future enhancement:

* schema-per-tenant model
* dedicated database tiers

---

## Tenant-Scoped Graph Intelligence

Azure Cosmos DB graph isolation through:

* tenant partition keys
* tenant-scoped graph traversal
* tenant-aware graph queries
* isolated relationship contexts

---

## Tenant-Scoped Storage

Blob storage separation through:

* tenant containers
* tenant prefixes
* RBAC enforcement
* SAS/token isolation

---

## Tenant-Scoped AI Context

AI investigation workflows must remain:

* tenant-scoped
* authorization-aware
* evidence-isolated
* context-bounded

LLM context assembly must never include:

* cross-tenant evidence
* unrelated tenant graph data
* unauthorized investigation context

---

# Identity & Authorization Strategy

The architecture adopts:

* Microsoft Entra ID integration
* claims-based authorization
* tenant-aware RBAC
* role-scoped investigations
* service-layer authorization enforcement

Potential roles:

* tenant administrator
* investigator
* compliance officer
* auditor
* executive viewer

---

# Security Considerations

Critical security principles:

## Tenant Context Enforcement

Every request must carry:

* tenant identity
* role claims
* authorization scope

---

## Service-Layer Enforcement

Tenant isolation must never rely solely on frontend enforcement.

All APIs and backend services must enforce:

* tenant filtering
* authorization boundaries
* scoped traversal logic

---

## Auditability

The platform must maintain:

* immutable audit logs
* tenant-scoped audit visibility
* access traceability
* investigation traceability

---

# Future Evolution

Potential future enhancements:

* dedicated enterprise tenant environments
* regional tenant isolation
* customer-managed encryption keys
* isolated ACA environments
* isolated AI inference boundaries
* sovereign cloud deployment models

---

# Consequences

## Positive Consequences

* scalable SaaS architecture
* Azure-native operational alignment
* strong enterprise positioning
* manageable operational complexity
* efficient shared-service operations
* clear tenant boundaries

---

## Negative Consequences

* tenant-awareness required across all services
* stronger authorization discipline required
* more complex testing strategy
* cross-tenant validation complexity
* operational governance overhead

---

# Related Architectural Areas

Related ADRs:

* ADR-0001 — Graph Database Selection
* ADR-0003 — AI Investigation Architecture
* ADR-0004 — Identity & RBAC Architecture
* ADR-0005 — Evidence Retention Strategy

---

# Summary

Integrity Sentinel adopts a:

> Shared-services, tenant-scoped isolation model.

The architecture emphasizes:

* Azure-native SaaS scalability
* strong tenant-aware authorization
* tenant-scoped graph intelligence
* isolated AI investigation context
* operational efficiency
* future enterprise extensibility
