# Figure 1.0: Enterprise Grid Telemetry & Dynamic Tariff Integration Engine
**Candidate:** Charity Ndlovu | Lead Systems Analyst Technical Submission  
**Architecture Archetype:** Event-Driven Kappa Streaming Architecture  

---

## 💎 Executive Architecture Summary
This repository contains the complete production-grade system blueprint, data models, and analytical queries designed to handle grid-scale smart meter telemetry ingestion and automated dynamic tariff calculation. 

To eliminate the system crashes and data loss common in legacy platforms, the architecture decouples the high-concurrency ingestion edge from active database lookup locks using a **Kappa Architecture** design pattern [aws_kinesis_data_streams_arch]. All processing, data validation, real-time alert baseline evaluations, and point-in-time financial re-billing tasks run through a single, unified immutable data stream ledger, ensuring absolute temporal data integrity.

---

## 📂 Repository Structural Blueprint & File Directory

### 🖼️ /models (System Blueprint Graphic Artifacts)
*   **`SystemsDesign.png` (Figure 1.0):** The macro enterprise architecture map displaying the asynchronous ingestion path through Amazon Kinesis, real-time AWS Lambda anomaly evaluation loops, and the isolated Databricks Medallion data lake zones [aws_kinesis_data_streams_arch]. It highlights the automated Gold Zone lookup loop triggered during `>350%` usage spikes.
*   **`UseCase.png` (Figure 2.0):** The micro-logic software boundary specification model utilizing strict UML oval elements. Maps automated system `<<include>>` pipelines and conditional `<<extend>>` exception workflows (e.g., automated workforce dispatch hooks).
*   **`ERD.png` (Figure 3.0):** The 3Third Normal Form normalized physical database schema. Outlines exact columns, data constraints, foreign key mappings, and high-performance time-series partitioning on the `readings.reading_date` field.
*   **`ActivityStateDiagram.png` (Figure 4.0):** A unified process workflow tracking how business calculation tasks trigger state-level changes on data rows (`UNPROCESSED` ➔ `BILLED` ➔ `RECONCILIATION`) when addressing late-data arrival lags.

### 📝 Core System Documentation Files
*   **`SQL.md`:** Contains four optimized window-calculation queries, standard deviation calculations for fraud detection, and point-in-time temporal joins to reconstruct historical billing states for late device logs.
*   **`TechnicalQuestions.md`:** Meticulous senior-level answers addressing five-year technical debt profiles, domain vs. application boundaries, and alignment with SAP condition pricing methodologies.
*   **`NonFunctionalRequirements.md`:** Outlines platform resilience, zero-trust data encryption, distributed audit tracking, and target load-testing parameters (50,000 requests/sec with <5s sink lag).
*   **`RoadmapPlanning.md`:** The strategic Quarter 1 engineering rollout timeline and architectural prioritization matrix.

---

## 🚀 Key Architectural Innovations Placed
1.  **Late-Data Alignment Engine:** Implements structural telemetry timestamps rather than ingestion clock stamps, eliminating calculation discrepancies caused by network latency dropouts.
2.  **Streaming Shock Absorber:** Leverages decoupled ingestion message streams to process heavy concurrent machine workloads cleanly [aws_kinesis_data_streams_arch].
3.  **Soft-Delete Configuration Invariance:** Enforces an append-only configuration layer with explicit date validity boundaries (`effective_start` / `effective_end`), protecting historical reporting files from schema truncation crashes.
configuration layer with explicit date validity boundaries (`effective_start` / `effective_end`), protecting historical reporting files from schema truncation crashes.
