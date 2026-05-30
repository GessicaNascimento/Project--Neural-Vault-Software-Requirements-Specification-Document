# Software Requirements Specification (SRS) — Neural Vault

## 1. Introduction & Methodological Framework
This document establishes the technical, architectural, and functional specifications for **Neural Vault**, an Enterprise Smart Memory and Knowledge Isolation Engine. The software design and requirements engineering processes follow the structured guidelines proposed by **Ian Sommerville (2007)**, ensuring traceability, structural integrity, and rigorous domain modeling.

The primary objective of Neural Vault is to mitigate institutional memory loss and solve corporate data fragmentation (occurring across isolated silos like chat logs, emails, and meeting transcripts) by deploying a secure **Retrieval-Augmented Generation (RAG)** architecture.

---

## 2. System Architecture Overview
The system is built upon a **Multi-Tier Layered Architecture**, separating client interfaces, application logic, and intelligent data persistence. 

1. **Client Tier:** A responsive web application managing ingestion triggers and natural language interfaces.
2. **Application Tier (FastAPI Engine):** An asynchronous Python framework handling request validation, sequential NLP pipelines, and the RAG semantic query retriever.
3. **Data & AI Persistence Tier:** A dual-engine subsystem integrating an isolated vertical Large Language Model (LLM), a dedicated Vector Database (**Qdrant / pgvector**) for semantic embeddings, and a Relational Database (**PostgreSQL**) for system metadata and auditing.

---

## 3. Functional Requirements (FR)
Functional requirements define the core capabilities and operational behaviors that the Neural Vault engine must execute.

| Requirement ID | Requirement Definition | Technical Context & Engineering Derivation |
| :--- | :--- | :--- |
| **FR01** | **Multi-Source Corporate Ingestion** | The system must automatically ingest unstructured data from Slack channels, Microsoft Teams, and Google Drive repositories. <br><br>*Derivation: Implement specialized API connectors utilizing secure OAuth 2.0 authentication protocols.* |
| **FR02** | **Asynchronous Audio Processing** | The system must ingest audio files from recorded corporate meetings and convert them into structured text files. <br><br>*Derivation: Integration with a specialized Speech-to-Text pipeline executing Whisper-based acoustic modeling.* |
| **FR03** | **Semantic Natural Language Querying** | The user must be able to query the organization's collective knowledge base using conversational natural language. <br><br>*Derivation: FastAPI endpoint routed to an embedding generator and a vector search similarity engine.* |
| **FR04** | **Role-Based Access Control (RBAC)** | The system must dynamically filter and restrict data retrieval based on the user's specific corporate role and clearance level. <br><br>*Derivation: Integration with Enterprise Identity Providers (IdP/Active Directory) to validate cryptographic tokens before database query compilation.* |

---

## 4. Domain & Security Restrictions (DR)
Domain restrictions represent non-negotiable structural rules imposed by the enterprise business environment, data privacy regulations, and safety boundaries.

| Restriction ID | Domain Boundary | Technical & Architectural Implementation |
| :--- | :--- | :--- |
| **DR01** | **Historical Data Immutability** | All ingested corporate evidence, communication logs, and historical meeting records cannot be modified or permanently deleted by regular users. <br><br>*Derivation: Enforce soft-delete strategies at the PostgreSQL database level combined with automated database append-only write locks.* |
| **DR03** | **Vertical AI Hallucination Shield** | The system is strictly forbidden from generating inferences or answers if the retrieved context does not meet high mathematical reliability thresholds. <br><br>*Derivation: Hardcoded threshold validation where Vector Search results must return a Cosine Similarity score $\ge$ 0.85. If the score is lower, the pipeline aborts generation and returns a standardized out-of-context error message.* |
| **DR05** | **Data Privacy Compliance (LGPD/GDPR)** | Personally Identifiable Information (PII) must not be stored inside the vector index or exposed to the foundational LLM engine. <br><br>*Derivation: Deploy a sequential Named Entity Recognition (NER) pipeline that intercepts text inputs, masking names, documents, and sensitive financial entities in real-time before vector indexing.* |

---

## 5. Non-Functional Requirements (NFR)
Non-functional requirements dictate the operational constraints, performance thresholds, and quality characteristics of the system.

* **NFR-01 (Security & Transport):** All data transport across tiers must be encrypted using HTTPS with TLS 1.3 protocols.
* **NFR-02 (Storage Security):** All database states, vector payloads, and relational tables must be encrypted at rest using AES-256.
* **NFR-03 (Performance & Concurrency):** The Application Tier must support high concurrency, handling up to 500 simultaneous query requests with a semantic response latency under 2.5 seconds, achieved via FastAPI native asynchronous loops.
