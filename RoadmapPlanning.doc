# Strategic Component Timeline & Rollout Justification
**Portfolio Module: Agile Product Roadmap & Milestones**
**Architecture Archetype: Kappa Streaming Engine**

---

## 1. Strategic Quarter 1 Engineering Roadmap
* **WEEKS 1–3: Core Storage Foundations & Schema Initialisation**
  ↳ Deploy the S3 cloud data lake containers and build out the normalized 3Third Normal Form relational schema structures for the Customers, Meters, Tariffs, and Readings tables.
* **WEEKS 4–6: Kappa Streaming Buffer Pipeline Deployment**
  ↳ Deploy secure Amazon Kinesis Data Streams and configure automated AWS Lambda streaming Ingestion paths to establish a continuous un-broken data highway [aws_kinesis_data_streams_arch].
* **WEEKS 7–9: Analytical Computation Engine Implementation**
  ↳ Build out the core point-in-time calculation engine logic, integrate the slowly changing dimension lookup matrices, and deploy the automated anomaly flagging filters.
* **WEEKS 10–12: Integration, Load Testing, & Executive Sign-Off**
  ↳ Execute distributed peak load testing runs, integrate customer-facing web APIs, complete end-to-end security audits, and secure executive deployment approval.

---

## 2. Component Rollout Priority Justification
The Q1 timeline prioritizes building out the data architecture and streaming ingestion loop during the first six weeks before developing any customer web applications or front-end user interfaces. In high-scale utility environments, trying to build features on top of an unstable data layer is a critical mistake. Securing a normalized, partitioned table layout upfront ensures the underlying core database can scale to handle massive data loads effortlessly. This solid data foundation allows downstream components, reporting engines, anomaly alert workflows, and billing modules to pull reliable data without needing costly changes to the system layout later on.
