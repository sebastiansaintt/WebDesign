# LawSim Pymes
### Enterprise Architecture & Semantic Web Design for Regulatory Compliance

Welcome to the technical design and architectural specifications repository for **LawSim Pymes**. This project is structured as an advanced **Software Architecture Case Study** and **Technical Design Document (TDD)**. It outlines a highly scalable, semantic web solution aimed at mitigating regulatory compliance risks for Small and Medium Enterprises (PyMEs) in Colombia.

---

## Case Study Context & The Business Problem

Operating a business in Colombia involves navigating a highly dynamic, complex, and fast-paced legislative environment. According to **Confecámaras (2023)**, a massive percentage of PyMEs unknowingly violate new regulations simply due to a lack of awareness or deep legal expertise. 

### The Cost of Non-Compliance:
*  **Severe Financial Sanctions:** Heavy fines that can drain a small company's working capital.
* **Operational Paralysis:** Full or partial shutdowns due to failure to meet newly enforced criteria.
* **Reactive Overhead:** Outsized expenses trying to adjust business operations hurriedly after an infraction is caught.
* **Competitive Disadvantage:** Inability to tender for contracts or maintain partnerships due to compliance gaps.

**LawSim Pymes** addresses this by shifting corporate compliance from a **reactive crisis** to an **automated, proactive strategy** through an ontological simulation engine.

---

## High-Level Architectural Pattern
The system is built on a hybrid architectural blueprint combining a **Layered Architecture**, a decoupled **Microservices Ecosystem**, and the **Model-View-Controller (MVC) pattern**. This layout ensures a complete separation between frontend presentation, business automation workflows, and the heavy automated reasoning engine.

![System Architecture](projectArchitecture.png)

### Architectural Rationale:
* **Separation of Concerns (Layered):** The complex semantic reasoning layer operates completely independently of user presentation and data persistence pipelines.
* **Independent Scalability (Microservices):** The computation-heavy ontology simulation engine scales horizontally during high-demand regulatory deadlines without forcing a replication of standard lookup services.
* **Technology Heterogeneity:** Seamlessly integrates a python-driven semantic sub-layer (`Apache Jena` / `Owlready2`) with robust `Node.js + Express` API gateways and a reactive `Vue.js` frontend.
* **Resilience & Fault Tolerance:** If the main regulatory ingestion microservice drops, the simulation module continues running using decoupled, cached datasets via `Redis`.

---

## Semantic Knowledge Graph & Inference Engine

At the core of LawSim Pymes lies an intricate **OWL 2 Ontology** designed within **Protégé 5.6**. Instead of mapping data into rigid rows and columns, the domain is modeled as a living knowledge graph capable of discovering non-obvious regulatory impacts.

### Knowledge Schema Layout:
* **Core Classes:** `Norma`, `Requisitos`, `CategoriaNormativa`, `PyME`, `Impacto`, `SeveridadImpacto`, `AccionCorrectiva`, `Simulacion`, `EscenarioSimulado`.
* **Sub-Class Contexts:** Structural categorization of laws (e.g., *Laboral*, *Ambiental*, *Tributaria*) and strict damage metrics (*Alta*, *Media*, *Baja*).
* **Object Properties (Inter-relations):** * `tieneRequisito` (Norma → Requisito)
  * `generaImpacto` (Requisito → Impacto)
  * `tieneSeveridad` (Impacto → SeveridadImpacto)
  * `requiereAccion` (Impacto → AccionCorrectiva)

### Automated Semantic Inference (SWRL Rules):
The platform integrates the **HermiT Reasoner** to compute implicit data automatically. Using Semantic Web Rule Language (SWRL), the system dynamically labels compliance breaches. For instance:

$$\text{PyME}(?p) \land \text{empleadosPyME}(?p, ?e) \land ?e > 50 \land \text{evalua}(?p, ?n) \land \text{nivelCriticidad}(?n, \text{"Crítico"}) \rightarrow \text{ImpactoSevero}(?p)$$

*Translation:* If a PyME is categorized as medium-sized (>50 employees) and evaluates a law flagged with critical regulatory weight, the system bypasses manual entry and explicitly infers a **High Severity Impact**, triggering real-time mitigation protocols.

---

## Interaction & Data Flows

### Semantic Calculation Process
The simulator evaluates an enterprise profile against regulatory changes using a systematic pipeline, determining breaches and compiling corrective strategies before saving states.

![Semantic Calculation Flow](flowDiagram.png)

### System Component Interactions
The sequence diagram demonstrates how components collaborate asynchronously: the Web Interface initiates requests, the Controller queries microservices, and the Semantic Engine executes logic on the Ontology before returning the calculated impact to `SimulacionService`.

![Sequence Interaction](sequenceDiagram.png)

---

## Decoupled Microservices Layout (SOAP / WSDL)

To maintain strict contract enforcement and technological decoupling, backend data pipelines utilize formalized web service descriptors.

![UML Service Components](components.png)

### 1. NormaService (Regulatory Data Repository)
Responsible for structural, read-heavy compliance navigation across mapped entities.
* `getNormas()`
* `getNormasPorCategoria(categoria)`
* `getNorma(id)`
* `getRequisitos(idNorma)`

### 2. SimulacionService (Scenario Processing Engine)
Handles analytical persistence, user workflows, and state records.
* `registrarSimulacion(pymeId, normaId, impactoGeneral)`
* `obtenerSimulaciones(pymeId)`
* `obtenerSimulacion(idSimulacion)`

---

## Web Modeling & Navigation (UWE & OOHDM)

The user experience avoids exposing non-technical users to dense legal jargon by leveraging structured modeling frameworks (**UWE & OOHDM**).

* **UWE (UML-Based Web Engineering):** Dictates user-centric content structure, establishing custom views depending on whether an auditor, business owner, or validator accesses the platform.
* **OOHDM (Object-Oriented Hypermedia Design Method):** Decouples abstract interface elements (e.g., compliance meters and severity dashboards) from background database models.
* **Navigational Ecosystem:**
  * **Index Structures:** Structured search trees (`PyME` → `Normas por Categoría` → `Norma específica`).
  * **Guided Tours:** A linear wizard taking users from corporate onboarding to scenario creation, results compilation, and structured execution timelines.

---

## System Requirements Specification

### Functional Architecture (Core Scope)

| ID | Requirement Specification | Priority | Operational Acceptance Criteria |
| :--- | :--- | :---: | :--- |
| **RF01** | Full catalog discovery of active regulations | High | Filterable lists sorting records by industry sector, date, and category. |
| **RF02** | Structural breakdowns of target laws | High | Deep-dive views splitting legal texts into distinct actionable articles. |
| **RF03** | Automated real-time profile alerts | Medium | Outbound email notifications delivered within 24 hours of matching law updates. |
| **RF05** | Financial simulation algorithm | High | Contextual cost-bracket calculations calibrated to company sizes. |
| **RF06** | Adaptation Gantt chart compilation | High | Auto-populated timelines showing explicit deadlines mapped to legal cutoff dates. |
| **RF10** | Enterprise profile configuration | High | Standardized company ledger recording NIT, industry code (CIIU), and sizing metrics. |
| **RF11** | Real-time DIAN API ledger validation | Medium | Automated profile population upon verification of a valid corporate taxpayer ID. |
| **RF13** | Validation & auditing workflows | Medium | Approval/rejection system with comments for consulting legal authorities. |
| **RF14** | Zero-downtime ontology deployment | High | Live hot-reloading mechanism for shipping schema updates without system outages. |

### Non-Functional Performance & Guardrails

| ID | Engineering Focus | Target Threshold / Specification | Architectural Justification |
| :--- | :--- | :--- | :--- |
| **RNF01** | Frontend Response | $< 3.0$ seconds | Guarantees fluid interaction across common page views. |
| **RNF02** | Simulation Speed | $\le 8.0$ seconds under peak load | Allocates budget for advanced ontology traversal and HermiT reasoning. |
| **RNF03** | Concurrency Budget | $100$ active simultaneous sessions | Meets traffic expectations for regional operational peaks. |
| **RNF05** | Auth Protocol | `JWT` + `RS256` (Asymmetric) / 5 min expiry | Secures sensitive corporate records and compliance histories. |
| **RNF06** | Data Encryption | `AES-256` at-rest encryption | Prevents breach leakage of underlying tax, ledger, and legal profiles. |
| **RNF08** | Document Trust | Cryptographic signatures on PDFs | Provides verifiable authenticity to generated legal and financial reports. |
| **RNF10** | Accessibility | `WCAG 2.1 AA` / 4.5:1 Contrast ratio | Ensures inclusivity across non-technical and varied user demographics. |
| **RNF13** | Infrastructure Scale| Horizontal Auto-scaling | Handles traffic spikes during critical corporate tax and reporting deadlines. |
| **RNF14** | Cache Topology | Distributed `Redis` instance | Minimizes database hits for highly repetitive regulatory definitions. |

---

## Tech Stack Portfolio Breakdown
* **Presentation View:** Vue.js, TailwindCSS
* **Orchestration / REST APIs:** Node.js, Express.js
* **Semantic Processing Core:** Python, Owlready2, Apache Jena
* **Automated Inference Reasoning:** HermiT Semantic Reasoner
* **Natural Language Processing (NLP):** LEGAL-BETO Language Model
* **Data Layer Topology:** PostgreSQL (Relational Core), MongoDB (Audit Logs), Redis (Distributed Caching)
