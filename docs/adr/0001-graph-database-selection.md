# ADR-0001 — Graph Database Selection

* Status: Proposed
* Date: 2026-05-13
* Decision Owner: Integrity Sentinel Architecture
* Category: Data Architecture

---

# Context

Integrity Sentinel is envisioned as an enterprise integrity intelligence platform focused on:

* relationship intelligence
* operational trust analysis
* behavioral analytics
* fraud detection
* insider risk monitoring
* AI-assisted investigations

A core architectural capability of the platform is the ability to model and analyze relationships between operational entities such as:

* employees
* managers
* vendors
* approvals
* transactions
* devices
* identities
* documents
* investigations

The platform requires support for:

* connected entity traversal
* relationship analysis
* graph-based anomaly detection
* collusion discovery
* influence analysis
* operational context enrichment
* AI-assisted evidence correlation

The architecture must also align with:

* Azure-native deployment
* SaaS multi-tenancy
* operational scalability
* explainable investigations
* hybrid transactional + analytical workloads

---

# Problem Statement

Integrity Sentinel requires a graph-oriented data capability that supports:

1. Operational entity storage
2. Relationship traversal
3. Graph analytics
4. AI investigation workflows
5. Scalable ingestion
6. Tenant isolation
7. Integration with Azure-native services

The architecture must determine:

* whether a dedicated graph database is required,
* whether graph capabilities should coexist with relational storage,
* and which Azure-aligned technologies provide the best balance of:

  * flexibility
  * scalability
  * operational complexity
  * cost
  * developer productivity

---

# Architectural Considerations

## Workload Characteristics

### Transactional Workloads

Examples:

* case management
* alert persistence
* tenant configuration
* policy management
* RBAC
* workflow state

These workloads are relational and transactional.

---

### Graph Workloads

Examples:

* relationship traversal
* approval-chain analysis
* collusion detection
* connected-entity discovery
* influence analysis
* behavioral context expansion
* graph-assisted investigations

These workloads benefit from graph-oriented storage and traversal.

---

### Analytical Workloads

Examples:

* anomaly scoring
* behavioral baselines
* aggregate analytics
* trend analysis
* executive dashboards

These workloads may require analytical or hybrid storage patterns.

---

# Evaluated Options

## Option A — Azure SQL Only

### Description

Use Azure SQL Database as the primary operational and relationship store.

Relationships modeled using:

* foreign keys
* adjacency tables
* recursive queries
* materialized relationship tables

---

### Advantages

* simplest operational model
* strong transactional consistency
* mature tooling
* low architectural complexity
* excellent operational familiarity
* lower initial cost

---

### Disadvantages

* limited graph traversal capability
* complex recursive relationship queries
* reduced performance for multi-hop traversals
* poor fit for relationship-centric investigations
* difficult graph analytics expansion
* weak semantic relationship modeling

---

### Assessment

Suitable for:

* early transactional prototype
* limited relationship depth
* small-scale operational workflows

Not ideal for:

* long-term graph intelligence platform
* advanced relationship analytics
* complex collusion detection

---

# Option B — Azure Cosmos DB (Gremlin API)

### Description

Use Azure Cosmos DB with Gremlin graph API as the primary graph intelligence layer.

Azure SQL remains the transactional operational system.

Cosmos DB stores:

* entities
* relationships
* graph traversals
* investigation context
* connected operational intelligence

---

### Advantages

* Azure-native managed service
* graph traversal support
* globally scalable
* multi-region capable
* good Azure integration
* suitable for SaaS isolation models
* supports relationship-centric investigations
* operationally aligned with Azure ecosystem

---

### Disadvantages

* Gremlin ecosystem less mature than Neo4j
* graph querying experience less ergonomic
* limited advanced graph analytics ecosystem
* developer experience complexity
* graph visualization tooling weaker than Neo4j ecosystem

---

### Assessment

Strong candidate for:

* Azure-native SaaS architecture
* operational graph intelligence
* scalable entity relationship modeling
* moderate graph complexity
* managed cloud operations

Potentially weaker for:

* highly advanced graph science workloads
* heavy graph algorithm research

---

# Option C — Neo4j

### Description

Use Neo4j as the dedicated enterprise graph platform.

Azure SQL remains the operational transactional store.

Neo4j stores:

* entities
* relationships
* graph analytics context
* investigation traversal data

---

### Advantages

* industry-leading graph database ecosystem
* strong Cypher query language
* superior graph developer experience
* advanced graph analytics capabilities
* excellent graph visualization tooling
* mature graph-science ecosystem
* ideal for relationship-centric intelligence

---

### Disadvantages

* additional operational complexity
* additional infrastructure management
* increased platform cost
* reduced Azure-native simplicity
* more complex SaaS operational model
* potential architectural overhead for early-stage platform

---

### Assessment

Excellent candidate for:

* advanced graph analytics
* relationship intelligence platforms
* graph-heavy operational intelligence
* mature enterprise graph workloads

Potential downside:

* may be operationally excessive for initial MVP stages

---

# Option D — Hybrid Azure SQL + Cosmos DB + Analytical Layer

### Description

Use:

* Azure SQL for transactional operational data
* Cosmos DB Gremlin for relationship intelligence
* analytical services for behavioral scoring and AI workflows

This separates:

* operational persistence
* graph intelligence
* analytical processing

---

### Advantages

* clear workload separation
* Azure-native alignment
* scalable architecture
* strong SaaS alignment
* flexible future evolution
* easier incremental implementation
* preserves relational strengths
* enables graph expansion

---

### Disadvantages

* increased architectural complexity
* cross-system synchronization concerns
* eventual consistency considerations
* more complex ingestion pipeline

---

### Assessment

Strong long-term architectural fit.

Provides:

* operational flexibility
* scalable graph intelligence
* cloud-native deployment alignment
* future analytical extensibility

---

# Decision

## Selected Direction

The proposed architecture adopts:

> Azure SQL Database + Azure Cosmos DB (Gremlin API)

as the primary operational and graph intelligence architecture.

---

# Decision Rationale

## Azure SQL Responsibilities

Azure SQL will serve as:

* transactional operational system
* case management store
* tenant configuration store
* policy store
* RBAC persistence layer
* workflow persistence layer
* operational reporting source

Azure SQL is strongly aligned with:

* transactional consistency
* operational workflows
* SaaS administration
* structured reporting

---

## Azure Cosmos DB Responsibilities

Azure Cosmos DB (Gremlin API) will serve as:

* enterprise integrity graph
* relationship intelligence layer
* connected-entity traversal engine
* investigation context graph
* operational relationship store

Cosmos DB is aligned with:

* relationship-centric analysis
* scalable graph traversal
* Azure-native SaaS architecture
* operational graph intelligence

---

# Why Neo4j Was Not Initially Selected

Neo4j remains a strong future candidate.

However, the current architectural direction prioritizes:

* Azure-native operational simplicity
* managed cloud services
* SaaS operational alignment
* reduced operational overhead
* incremental platform evolution

Neo4j may later become appropriate if:

* graph workloads become dominant,
* advanced graph-science requirements emerge,
* or dedicated graph analytics capabilities significantly expand.

---

# Consequences

## Positive Consequences

* Azure-native architecture consistency
* scalable SaaS alignment
* clear workload separation
* flexible future evolution
* manageable operational complexity
* strong relationship-analysis foundation

---

## Negative Consequences

* dual persistence architecture complexity
* ingestion synchronization requirements
* graph traversal limitations compared to Neo4j
* additional operational coordination

---

# Future Considerations

Potential future enhancements:

* graph ML integration
* vector-enhanced graph retrieval
* graph-assisted LLM reasoning
* dedicated graph analytics services
* graph visualization workbench
* hybrid Cosmos DB + Neo4j evolution

---

# Related Architectural Areas

Related ADRs:

* ADR-0002 — Tenant Isolation Strategy
* ADR-0003 — AI Investigation Architecture
* ADR-0004 — Event-Driven Ingestion Architecture
* ADR-0005 — Vector Retrieval Architecture

---

# Summary

Integrity Sentinel adopts a hybrid architecture using:

* Azure SQL Database for transactional operational workflows
* Azure Cosmos DB (Gremlin API) for enterprise relationship intelligence

This architecture balances:

* Azure-native simplicity
* SaaS scalability
* relationship-centric analysis
* operational manageability
* future graph intelligence evolution
