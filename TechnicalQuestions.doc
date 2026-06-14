# Core System Analysis & Architectural Tradeoffs
**Portfolio Module: Comprehensive Architectural Evaluations**
**Architecture Archetype: Kappa Streaming Engine**

---

## 1. System Behaviour upon New Configuration Insertion
When a commercial analyst inserts a new tariff profile row into the `Tariffs` table, the platform adapts dynamically without requiring manual software deployments. In the decoupled Kappa Architecture engineered for this submission, the data pipeline treats configurations as an active metadata lookup catalog [aws_kinesis_data_streams_arch]. On the next execution run, the streaming analytical layer reads the latest catalog state. Incoming daily usage rows are evaluated against this new configuration automatically based on the row's `effective_start` date boundary, ensuring immediate operational updates with zero platform downtime.

---

## 2. Five-Year Technical Debt Projection
Running this high-volume utility platform continuously for five years without infrastructure maintenance will expose predictable database gravity and technical debt bottlenecks:
* **Transactional Data Gravity:** The `Readings` table will capture billions of rows, slowing down lookups if time-based partitioning boundaries are set too wide.
* **Schema Drift and Firmware Variances:** Legacy smart meter models pushing outdated or modified JSON formats will fail incoming validation, causing a surge in logs dumped into the Dead Letter Queue.
* **Tariff Dimension Fragmentation:** Frequent back-dated contract edits will fragment the Slowly Changing Dimension (SCD Type 2) tables, extending point-in-time calculation query latencies.

---

## 3. Engineering Tradeoffs: Velocity vs. Quality
* **Tactical Approach (High Speed / Low Quality):** Constructing a single, monolithic flat table would allow a client web application to launch within weeks. However, this approach completely destroys point-in-time financial auditing, drastically inflates ongoing cloud storage compute costs, and limits the platform's ability to run advanced analytics.
* **Strategic Approach (Intentional Quality):** Implementing the decoupled Kappa data lakehouse layout requires an upfront engineering commitment over a full quarter [aws_kinesis_data_streams_arch]. However, it guarantees sub-second query execution, establishes an immutable audit log, and aggressively lowers long-term compute overhead.

---

## 4. Addressing the Specific Technical Debt of Late-Data Arrivals
The most significant technical debt challenge in high-scale grid metering is managing late-data arrivals caused by cell tower dropouts or physical device blackouts. If a meter pushes its historical logs five days late, a standard system mistakenly stamps it with today's date and today's active price, causing incorrect customer billing. This challenge is solved by enforcing explicit temporal data logic within the lakehouse layout: parsing payloads by their structural telemetry log date rather than their physical ingestion arrival timestamp, isolating temporal deltas, and routing delayed records into a designated `RECONCILIATION` state for retroactive billing correction.

---

## 5. Domain Logic vs. Application Logic
* **Domain Logic:** High-value, unchanging business rules that remain true regardless of the cloud infrastructure or programming language used. Examples include calculating usage cost (`kWh * Price`), verifying contract validity dates, and evaluating meter variance theft spikes `>350%`.
* **Application Logic:** The operational code infrastructure required to execute and host the platform. Examples include configuring AWS Lambda streams, managing Kinesis data stream buffer sizes, parsing JSON payloads, and configuring database indexing protocols [aws_kinesis_data_streams_arch].

---

## 6. Configurable Data vs. Hard-Coded Logic
* **Configurable Data:** Dynamic parameters stored in external lookup tables that can be updated on demand by business users without changing code. Examples include cost per kilowatt-hour, active contract dates, load shedding log dates, and anomaly variance percentage thresholds.
* **Hard-Coded Logic:** Structural, algorithmic foundations written directly into the source repository that require full engineering code reviews to change. Examples include the platform's SHA-256 customer pseudonymisation routine, user access authentication flows, and the Medallion data lake transition sequences [aws_kinesis_data_streams_arch].

---

## 7. Architectural Alignment with SAP Condition Pricing Archetypes
The architecture directly mirrors the condition table methodology utilized by enterprise SAP billing systems (replicating `VK11/VK12` table patterns). SAP separates high-frequency logistic actions from price determination records. This design follows that pattern exactly: consumption events are captured as lean, numeric records in an isolated `Readings` table, while the decoupled `Tariffs` table serves as the independent pricing condition layer. The final billing cost is never hard-coded; it is determined by running a point-in-time evaluation matrix joining these tables on demand.

---

## 8. The Operational Danger of Configuration Data Deletion
Deleting a row from the configuration `Tariffs` table is a severe database engineering failure. If a user purges an old plan profile, any past historical `Readings` rows linked to that plan lose their foreign key targets. When the system re-runs financial audits or calculates rebates, the missing lookups will cause background queries to crash or force an unintended fallback to a base flat rate. To prevent this, the platform enforces an Append-Only Soft Delete Pattern: pricing records are never removed; instead, their availability windows are closed by updating their `effective_end` date column.

---

## 9. Modern Utility Data Classification Matrix
* **Configuration Data:** Low-volume, high-value, slowly changing structural metadata rules that dictate system behavior and pricing guidelines (e.g., Tariffs table parameters, baseline threshold constraints).
* **Transactional Data:** High-volume, append-only operational telemetry logs generated continuously by external physical events (e.g., 15-minute smart meter readings, raw streaming queue records).
* **Derived Data:** Calculated mathematical outputs produced by executing domain logic across primary data elements (e.g., customer monthly statements, aggregated load profiles, historical simulation deltas).

---

## 10. High-Scale Data Growth Profiles
The `Readings` table experiences an exponential growth profile compared to all other system databases. A fleet of 1 million smart meters transmitting 15-minute telemetry intervals will generate 35 million new rows every single day, or over 12 billion records a year. This data gravity can cause querying speeds to degrade rapidly. This is mitigated by establishing a strict time-based partitioning strategy directly on the `reading_date` column, supported by an automated data lifecycle rule that transfers historical database records older than 24 months into low-cost cloud cold storage.
 automated data lifecycle rule that transfers historical database records older than 24 months into low-cost cloud cold storage.
