# Technical Data Requirements and Scoping Template

> **Purpose:** Complete this template for each data source or data product before delivery dates are committed. It turns a mapped source into an engineering-ready scope, data contract, dependency plan, and definition of done.
>
> **How to use:** BCG / Platinion facilitates completion with Business, the source-system team, Integration / Platform, BI, and Data Engineering (DE). DE approves technical feasibility; do not treat unanswered fields as build-ready assumptions.
>
> **Status:** Draft / In discovery / Ready for DE review / Approved for build / Blocked
>
> **Document owner:** [Name and team]
>
> **Last updated:** [YYYY-MM-DD]

---

## 1. Scope and Ownership

| Field | Response |
|---|---|
| Use case / data product name | [Name] |
| Business outcome enabled | [Decision, process, model, or report enabled] |
| Scope in | [Explicit list] |
| Scope out | [Explicit list] |
| Business Product Owner | [Name, team] |
| Business SME / acceptance approver | [Name, team] |
| Source-system owner | [Name, team] |
| Source technical contact | [Name, team] |
| Integration Platform owner, if applicable | [Name, team] |
| Data Engineering owner | [Name, team] |
| Data Steward / governance owner | [Name, team] |
| BI / analytics consumer owner | [Name, team] |

## 2. Business and Consumer Requirements

| Question | Response |
|---|---|
| What business decision, process, or analysis will this support? | [Response] |
| Who consumes the data? | [Teams, roles, applications] |
| What output is required? | [Dashboard, model, curated table, semantic view, extract, API] |
| What is the required business grain? | [Example: one record per trade, product/location/day] |
| Required history / backfill | [None, period, source availability] |
| Refresh frequency and required availability time | [Example: daily by 07:00 CET] |
| Maximum acceptable latency | [Example: 24 hours] |
| Required measures, dimensions, and business definitions | [Link or list] |
| Required reconciliation to source | [Totals, record counts, tolerance, approver] |
| Consumer acceptance criteria | [3-7 observable, testable conditions] |

## 3. Source Inventory and Extraction Method

| Field | Response |
|---|---|
| Source system | [System name] |
| Source objects | [Tables, views, reports, APIs, files, topics, CDC logs] |
| System of record? | [Yes / No; describe authoritative source if no] |
| Source environment for development / test / production | [Details] |
| Extraction method chosen | [Integration platform / API / CDC / replication / file drop / SFTP / message queue / other] |
| Why is this pattern appropriate? | [Security, capability, latency, volume, enterprise standard] |
| Alternatives considered and rejected | [Option and rationale] |
| Full load, incremental, or both? | [Details] |
| Incremental mechanism | [Watermark, CDC token, timestamp, sequence, event offset] |
| How are updates and deletions represented? | [Details] |
| Retry, replay, and idempotency behavior | [Details] |
| Extract schedule and source-system release / blackout windows | [Details] |
| API limits, pagination, export constraints, or known limitations | [Details] |

### Ingestion Decision

| Decision | Selected option | Approver | Date | Notes |
|---|---|---|---|---|
| Enterprise integration platform used? | [Yes / No / N/A] | [Name] | [Date] | [Rationale] |
| Separate ingestion component required? | [Yes / No] | [Name] | [Date] | [Rationale] |
| Architecture review required? | [Yes / No] | [Name] | [Date] | [Reference] |
| Source-system change required? | [Yes / No] | [Name] | [Date] | [Description] |

## 4. Connectivity, Security, and Access

| Requirement | Response |
|---|---|
| Authentication method | [OAuth/OIDC, managed identity, certificate, service account, other] |
| Credential / service-account owner | [Name, team] |
| Secret / certificate storage and rotation | [Location, owner, cadence] |
| Network route | [Private endpoint, VPN, integration runtime, firewall path] |
| Firewall, DNS, private endpoint, or allow-list changes | [Details and owner] |
| Required source-system permissions | [Read roles, API scopes, file access] |
| Data classification | [Public / Internal / Confidential / Restricted] |
| PII, commercially sensitive, or regulated fields | [Fields and classification] |
| Required masking, row filtering, or column restrictions | [Details] |
| Retention, legal hold, residency, or audit requirements | [Details] |

## 5. Incoming Data Contract

| Field | Response |
|---|---|
| Delivery format | [Delta, Parquet, CSV, Excel, JSON, XML, Avro, API payload, other] |
| Delivery location / endpoint / topic | [Reference; do not include credentials] |
| Encoding, delimiter, locale, and decimal convention | [Details] |
| Date, time, timezone, and timestamp convention | [Details] |
| Expected initial load volume | [Records, size] |
| Expected ongoing volume and peak volume | [Records/files/events per interval] |
| Expected growth rate | [Estimate] |
| Primary business key | [Field(s)] |
| Technical identifier / source record identifier | [Field(s)] |
| Nested structures / arrays / structs | [List and expected cardinality] |
| Schema evolution policy | [Add / rename / remove fields; notification period] |
| Sample data supplied | [Link and date] |
| Edge cases supplied | [Nulls, duplicate, correction, deletion, malformed examples] |

### Source-to-Target Mapping

Repeat this table for all source fields, or link to a managed mapping workbook.

| Source object / field | Source type | Nullable | Business meaning | Target layer / table / field | Target type | Transformation / validation rule | Required? | Owner |
|---|---|---|---|---|---|---|---|---|
| [Example: orders.order_date] | [timestamp] | [Yes/No] | [Meaning] | [silver.sales_orders.order_date] | [date] | [Convert UTC to business date] | [Yes/No] | [Name] |
| [Add row] |  |  |  |  |  |  |  |  |

## 6. Bronze to Silver Processing Requirements

| Question | Response |
|---|---|
| Raw data retained unchanged? | [Yes / No; explain exceptions] |
| Bronze location / table and retention period | [Details] |
| Silver target table(s) | [Catalog.schema.table] |
| Cleansing and standardisation rules | [Names, types, units, currency, timezone, code values] |
| Deduplication rule | [Key, precedence, surviving record] |
| Joining / reference-data requirements | [Master data, hierarchy, lookup, owner] |
| Derived fields and calculation rules | [Definitions and owner] |
| Slowly changing / history handling | [Type, effective dates, correction behavior] |
| Array / struct flattening or splitting rule | [Parent-child grain and key propagation] |
| Invalid-record handling | [Reject / quarantine / load with warning; owner and SLA] |
| Reprocessing and backfill approach | [Scope, approval, expected duration] |

### Data-Quality Rules

| Rule | Layer | Threshold / expected value | Failure action | Owner |
|---|---|---|---|---|
| Freshness | [Bronze/Silver/Gold] | [Example: available by 07:00 CET] | [Alert / block downstream] | [Name] |
| Completeness | [Layer] | [Example: order ID not null] | [Action] | [Name] |
| Uniqueness | [Layer] | [Example: one record per order ID/version] | [Action] | [Name] |
| Validity | [Layer] | [Example: currency in approved ISO list] | [Action] | [Name] |
| Reconciliation | [Layer] | [Example: daily source total within 0.1%] | [Action] | [Name] |
| [Add rule] |  |  |  |  |

## 7. Silver to Gold and Consumer Data Contract

| Field | Response |
|---|---|
| Gold output object(s) | [Catalog.schema.table/view] |
| Consumer-facing grain | [One record per ...] |
| Required aggregation rules | [Measures, grouping dimensions, aggregation functions] |
| Required split / explode rules | [Arrays, child records, parent keys] |
| Required conformed dimensions | [Calendar, customer, product, location, legal entity, currency] |
| Required semantic definitions | [Metric and dimension definitions; link] |
| Output schema, field naming, and data-type requirements | [Details or link to mapping] |
| Consumer refresh SLA | [Frequency and availability deadline] |
| Consumer access groups and permissions | [Groups, purpose, steward approval] |
| Downstream dashboards, models, extracts, or systems | [Names and owners] |
| Versioning and deprecation approach | [Notification, coexistence, migration] |

## 8. Dependencies, Risks, and Timeline

| Dependency / risk | Required deliverable or decision | Upstream owner | Downstream owner | Required-by date | Status | Escalation path / next action |
|---|---|---|---|---|---|---|
| [Example: API read scope] | [Provision service account] | [Source system team] | [DE] | [YYYY-MM-DD] | [Open] | [Action] |
| [Add row] |  |  |  |  |  |  |

### Delivery Milestones

| Milestone | Target date | Accountable owner | Exit criteria |
|---|---|---|---|
| Discovery complete | [Date] | [Name] | [Source, owners, ingestion route, samples confirmed] |
| Technical design and data contract approved | [Date] | [DE Lead] | [Sections 1-7 completed; open assumptions accepted] |
| Source and access dependencies resolved | [Date] | [Name] | [Access and test data verified] |
| Engineering build complete | [Date] | [DE] | [Pipelines and transformations deployed to DEV] |
| QA and reconciliation complete | [Date] | [DE / Business SME] | [Quality rules pass and reconciliation signed] |
| Production ready | [Date] | [Product Owner / DE Lead] | [All definition-of-done checks pass] |

## 9. Operational Handover

| Field | Response |
|---|---|
| Pipeline support owner | [Name, team] |
| Source-feed support owner | [Name, team] |
| Data-quality issue owner | [Name, team] |
| Monitoring and alert recipients | [Distribution group / tool] |
| Incident priority and response expectation | [Details] |
| Runbook location | [Link] |
| Replay / rerun process | [Who can run it, approvals, expected impact] |
| Backfill process | [Who approves, approach, cost/performance limits] |
| Change-management process | [Schema, logic, source changes, notice period] |

## 10. Build Readiness and Sign-Off

Do not commit engineering build dates until every applicable item is complete or has an explicit accepted risk and owner.

| Readiness check | Status | Evidence / link | Approver |
|---|---|---|---|
| Business outcome, scope, and acceptance criteria agreed | [Pass / Fail / N/A] | [Link] | [Business Product Owner] |
| Source objects and source owner confirmed | [Pass / Fail / N/A] | [Link] | [Source owner] |
| Ingestion pattern selected and architecture-approved | [Pass / Fail / N/A] | [Link] | [DE / Architecture] |
| Source access, network path, and authentication verified | [Pass / Fail / N/A] | [Link] | [Source / Platform owner] |
| Representative sample data and edge cases received | [Pass / Fail / N/A] | [Link] | [DE] |
| Incoming data contract and schema-evolution policy agreed | [Pass / Fail / N/A] | [Link] | [Source owner / DE] |
| Source-to-target mapping complete | [Pass / Fail / N/A] | [Link] | [DE / Data Steward] |
| Quality, reconciliation, and invalid-record rules agreed | [Pass / Fail / N/A] | [Link] | [Business SME / DE] |
| Gold output, consumer contract, and access needs agreed | [Pass / Fail / N/A] | [Link] | [Product Owner / BI] |
| Dependencies have owners, dates, and escalation actions | [Pass / Fail / N/A] | [Link] | [BCG / Programme lead] |
| Operational ownership and support approach agreed | [Pass / Fail / N/A] | [Link] | [DE Lead] |

### Sign-Off

| Role | Name | Decision | Date | Comments / accepted risks |
|---|---|---|---|---|
| Business Product Owner | [Name] | [Approve / Reject] | [Date] | [Comments] |
| Source-System Owner | [Name] | [Approve / Reject] | [Date] | [Comments] |
| Data Engineering Lead | [Name] | [Approved for build / Not ready] | [Date] | [Comments] |
| Data Steward | [Name] | [Approve / Reject / N/A] | [Date] | [Comments] |
| BI / Analytics Lead | [Name] | [Approve / Reject / N/A] | [Date] | [Comments] |
| BCG / Platinion Programme Lead | [Name] | [Dependencies recorded / Not recorded] | [Date] | [Comments] |