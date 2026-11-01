<!-- Rapider Official App SOW & Specification | App #098: Data Integration / ETL (data-integration) | Domain: DAT -->

# #098 Data Integration / ETL (`data-integration`) — Scope of Work (SOW)

- **App Number & Slug:** `#098` / `098-data-integration-etl`
- **Proposed Rapider App Key:** `data-integration`
- **Suite / Domain Key:** `DAT` (Data, Analytics & Business Intelligence Suite — `Dat*` Canonical Prefix)
- **Representative Product (`P1`):** Fivetran (+ dbt Transformation Layer)
- **Combined Feature Catalog Reference:** [../combined-features/098-data-integration-etl-features.md](../combined-features/098-data-integration-etl-features.md)

---

## 1. Executive Outcome & Target Audience

`data-integration` is the automated ELT, Change Data Capture (CDC), Data Cleansing, and Warehouse-Native Transformation backbone of the Rapider `DAT` suite. Data teams waste up to 60% of engineering capacity maintaining brittle cron scripts that break whenever an upstream SaaS vendor adds a column, an API rate-limits requests, or an OAuth token expires. `data-integration` replaces fragile scripts with resilient, checkpointed sync pipelines (`DatSyncPipeline`), automated schema drift policies (`DatSchemaDriftEvent`), in-flight PII hashing, visual cleansing + dbt-style SQL transformation DAGs (`DatTransformationModel`), and Reverse ETL activation back into Rapider operational suites.

- **SME Fast-Path Defaults:** Business analysts and operations leads connect external SaaS apps, CSV/SFTP drops, or legacy databases in minutes using preflight-validated wizards and no-code visual cleansing recipes (date/currency normalization, deduplication) landing clean tables directly into `#099` (`data-warehouse`) and `#096` (`bi-dashboards`).
- **Enterprise Progressive Disclosure:** Data platform engineers unlock sub-minute log-based CDC, SCD Type 2 temporal history tracking, declarative custom REST/GraphQL connector specs, modular SQL transformation DAGs synced with `#101` (`source-control`), Monthly Active Rows (MAR) budget guardrails, and downstream BI circuit breakers.
- **Measurable Success Outcomes:**
  - `>= 99.9%` pipeline reliability via resumable LSN/cursor checkpointing (`DatReplicationCheckpoint`) and jittered exponential retries.
  - Zero unannounced pipeline breakages from safe upstream column additions via governed `DatSchemaDriftEvent` propagation rules.
  - Zero plaintext credentials stored in application records (100% vault secret reference enforcement).
- **Top 6 Global Benchmarks Synthesized:** `P1` Fivetran (+ dbt), `P2` Airbyte, `P3` Informatica IDMC, `P4` Microsoft Fabric Data Factory, `P5` Qlik Talend Cloud, and `P6` Zoho DataPrep.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### In-Scope
1. Vault-backed source connectors (`DatSourceConnection`) and destination landing targets (`DatDestinationTarget`) with non-destructive preflight diagnostics and custom declarative REST/GraphQL connector specs.
2. Multi-mode replication pipelines (`DatSyncPipeline`: full refresh, incremental append/upsert, log-based CDC, SCD Type 2 history) with atomic stream checkpoints (`DatReplicationCheckpoint`) and in-flight salted PII column hashing.
3. Automated schema drift detection and resolution workflows (`DatSchemaDriftEvent`) routed to Stewards via Home `WorkItem`s when breaking changes occur.
4. No-code visual cleansing transforms and post-load dbt-style SQL transformation DAGs (`DatTransformationModel`) with inline quality assertions and dead-letter quarantine routing.
5. Execution observability (`DatSyncRun`), freshness SLA monitors, MAR/cost spike circuit breakers, and Reverse ETL operational enrichment via canonical Rapider suite guarded commands.

### Out-of-Scope
1. Analytical SQL compute cluster provisioning, physical warehouse table storage management, and ad-hoc SQL worksheets — owned by `#099 Data Warehouse & Analytics Platform` (`data-warehouse`).
2. Executive BI charts, semantic KPI scorecards, and report bursting — owned by `#096 Business Intelligence & Dashboards` (`bi-dashboards`).
3. Enterprise business glossary stewardship and cross-system access request governance — owned by `#100 Data Governance & Catalog` (`data-governance`).
4. General-purpose human approval BPM forms or customer webhook iPaaS UI workflows — owned by Rapider platform `requiredWorkflows` / BPM.

### Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Single Source of Truth) | Integration Contract with `data-integration` (`#098`) |
| :--- | :--- | :--- |
| **`data-integration` (`#098`)** | `DatSourceConnection`, `DatDestinationTarget`, `DatSyncPipeline`, `DatReplicationCheckpoint`, `DatSyncRun`, `DatSchemaDriftEvent`, `DatTransformationModel` | Orchestrates extraction, CDC, cleansing, warehouse transformation triggers, and emits technical lineage & freshness events. |
| **`#099` Data Warehouse (`data-warehouse`)** | `DatDatabaseCatalog`, `DatWarehouseTable`, `DatComputeCluster` | `DatDestinationTarget` lands staging/mart tables into `#099` `DatWarehouseTable` and executes pushdown `DatTransformationModel` SQL on `#099` compute. |
| **`#100` Data Governance (`data-governance`)** | `DatLineageEdge`, `DatQualityIncident`, `DatClassificationTag` | Publishes column-level `DatLineageEdge` graphs after every DAG run and emits `DatQualityIncident` events when inline transformation tests quarantine rows. |
| **`#096` BI Dashboards (`bi-dashboards`)** | `DatSemanticModel`, `DatReportSubscription` | Emits `dat.etl.mart.refreshed.v1` and freshness SLA breach signals to trigger `#096` snapshot refreshes or hold stale executive digests. |
| **`#101` Source Control & `#102` CI/CD** | `DevRepository`, `DevPullRequest`, `DevPipelineRun` | Syncs SQL `DatTransformationModel` files with `#101` Git branches and validates PR dry-runs via `#102`. |
| **Operational Suites (`CRM`, `FIN`, `OPS`, `HR`, `MKT`)** | Canonical domain entities (`CrmAccount`, `OpsSupplierProfile`, `FinInvoice`, etc.) | Reverse ETL (`ETL-005`) invokes target suite guarded commands (never raw collection bypass) to enrich operational records safely. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

All visible sidebar sections enforce **`>= 3` visible items (`displayOnMenu: true`)**, while detail views and sliding creation/edit drawers set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `integration-hub` | Pipeline Health & SLA Hub | `Pipelines & Runs` | `true` (Order 1) | Freshness SLA compliance cards, 24h sync throughput chart, MAR budget gauge, active failure/drift `WorkItem` banner, `#activeFilterTagsBar`. |
| 2 | `sync-pipelines` | Sync Pipelines | `Pipelines & Runs` | `true` (Order 2) | `findSWR` table of `DatSyncPipeline`s with sync mode pills (`CDC`, `INCREMENTAL`, `SCD2`), clickable pipeline name, `Sync Now` action, icon+text `Edit`, icon-only `Delete`. |
| 3 | `sync-runs` | Sync Run Ledger | `Pipelines & Runs` | `true` (Order 3) | Execution history (`DatSyncRun`) filterable by `SUCCEEDED / FAILED / RUNNING / QUARANTINED`, rows synced, MAR delta, duration, and `#activeFilterTagsBar` + `Clear all`. |
| 4 | `source-connections` | Source Connectors | `Connectivity` | `true` (Order 4) | Registry of `DatSourceConnection`s showing connector type, vault secret status, OAuth expiry countdown, and `"Test Preflight"` button. |
| 5 | `destination-targets` | Destinations & Reverse ETL | `Connectivity` | `true` (Order 5) | Catalog of `DatDestinationTarget`s (`#099` warehouse catalogs, object staging, and Reverse ETL suite targets) with latency and write health. |
| 6 | `custom-connector-studio` | Custom API Connector Studio | `Connectivity` | `true` (Order 6) | Declarative REST/GraphQL connector builder (`ETL-004`) with pagination, rate-limit policy, JSONPath tester, and AI OpenAPI spec importer. |
| 7 | `transformation-models` | Transformations & Prep | `Transform & Governance` | `true` (Order 7) | Catalog of SQL & visual-prep `DatTransformationModel`s showing materialization (`VIEW / TABLE / INCREMENTAL`), quality test pass rate, and `#101` Git ref. |
| 8 | `dag-lineage-explorer` | Pipeline DAG Visualizer | `Transform & Governance` | `true` (Order 8) | Interactive end-to-end graph (`Source -> Sync -> Staging -> Mart -> BI Model`) with freshness overlays and downstream impact highlighting. |
| 9 | `schema-drift-center` | Schema Drift & Quarantine | `Transform & Governance` | `true` (Order 9) | `DatSchemaDriftEvent` queue and dead-letter quarantine inspector with column diff viewer and `Approve Propagation / Block Column / Hash PII` actions. |
| 10 | `pipeline-detail` | Pipeline Stream & Checkpoint Detail | *(Hidden)* | `false` | Per-table stream selector (`#/pipeline-detail?id=...`), PII column hash toggles, `DatReplicationCheckpoint` LSN/cursor state, surgical table resync, and `Back to Pipelines`. |
| 11 | `run-attempt-detail` | Sync Run Log & AI Diagnostics | *(Hidden)* | `false` | Structured attempt logs (`#/run-attempt-detail?id=...`), HTTP/CDC error inspector, `projectChannelMessage` incident thread, and AI Fix Assistant. |
| 12 | `connector-pipeline-drawer` | Connector & Pipeline Drawer | *(Hidden)* | `false` | Sliding creation/edit drawer for `DatSourceConnection`, `DatSyncPipeline`, and `DatTransformationModel`; enforces `try/catch/finally` button reset and `closePageDrawer()`. |

---

## 4. Canonical Data Models (`Dat*` — 7 Models)

All models strictly omit `projectId`, store zero plaintext credentials, use platform UUID `id` + optimistic `version`, and use `Decimal128` for cost/MAR quota budgets.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`DatSourceConnection`** | `id: UUID`, `code: string`, `name: string`, `connectorType: enum(POSTGRES, MYSQL, MSSQL, ORACLE, REST_API, GRAPHQL, S3_GCS, SFTP, SAAS_APP)`, `vaultSecretRef: string`, `customSpecJson: object?`, `preflightStatus: enum(UNTESTED, HEALTHY, DEGRADED, AUTH_EXPIRED)`, `oauthExpiresAt: datetime?`, `lastTestedAt: datetime?`, `ownerUserId: UUID`, `version: int` | Referenced by `DatSyncPipeline.sourceConnectionId`; `vaultSecretRef` resolves exclusively via Platform Secret Vault at worker runtime. | **Zero-secret invariant:** `customSpecJson` and all string fields are scanned on write to reject embedded passwords, bearer tokens, or connection URIs. |
| **`DatDestinationTarget`** | `id: UUID`, `code: string`, `name: string`, `targetType: enum(RAPIDER_WAREHOUSE, OBJECT_STORAGE_ICEBERG, REVERSE_ETL_SUITE)`, `warehouseCatalogId: UUID?`, `targetSuiteKey: string?`, `vaultSecretRef: string?`, `defaultSchemaPrefix: string`, `status: enum(ACTIVE, PAUSED, ERROR)`, `version: int` | `warehouseCatalogId -> DatDatabaseCatalog.id` (`#099`). | Reverse ETL targets (`REVERSE_ETL_SUITE`) must declare a valid Rapider suite guarded command contract. |
| **`DatSyncPipeline`** | `id: UUID`, `code: string`, `name: string`, `sourceConnectionId: UUID`, `destinationTargetId: UUID`, `syncMode: enum(FULL_REFRESH, INCREMENTAL_APPEND, INCREMENTAL_UPSERT, LOG_BASED_CDC)`, `historyModeScd2: boolean`, `streamsConfigJson: {streamName, enabled, primaryKeys, cursorField, hashedColumns, blockedColumns}[]`, `driftPolicy: enum(AUTO_PROPAGATE_NEW, SAFE_WIDENING_ONLY, QUARANTINE_AND_PAUSE)`, `scheduleCron: string`, `freshnessSlaMinutes: int`, `monthlyMarCap: Decimal128?`, `status: enum(ACTIVE, PAUSED, BLOCKED_BY_DRIFT, ERROR)`, `version: int` | `sourceConnectionId -> DatSourceConnection.id`; `destinationTargetId -> DatDestinationTarget.id`. | `INCREMENTAL_UPSERT` and `LOG_BASED_CDC` require non-empty `primaryKeys` per enabled stream. Concurrent execution on the same `DatSyncPipeline.id` is mutually excluded via distributed lease. |
| **`DatReplicationCheckpoint`** | `id: UUID`, `pipelineId: UUID`, `streamName: string`, `cursorType: enum(WAL_LSN, BINLOG_POS, TIMESTAMP_WATERMARK, API_CURSOR_TOKEN)`, `checkpointStateJson: object`, `lastCommittedAt: datetime`, `rowsTotalSynced: int`, `version: int` | `pipelineId -> DatSyncPipeline.id`. | Checkpoints advance atomically only after target staging batch commit succeeds, guaranteeing at-least-once delivery with idempotent deduplication. |
| **`DatSyncRun`** | `id: UUID`, `pipelineId: UUID`, `triggerType: enum(SCHEDULED, MANUAL, WEBHOOK, BACKFILL)`, `status: enum(QUEUED, RUNNING, SUCCEEDED, PARTIAL_QUARANTINE, FAILED, CANCELED)`, `startedAt: datetime`, `completedAt: datetime?`, `rowsExtracted: int`, `rowsUpserted: int`, `rowsQuarantined: int`, `marCounted: Decimal128`, `attemptNumber: int`, `errorCode: string?`, `errorSummary: string?`, `discussionChannelId: UUID?`, `activeWorkItemId: UUID?`, `idempotencyKey: string` | `pipelineId -> DatSyncPipeline.id`; `activeWorkItemId -> Platform WorkItem.id`; `discussionChannelId -> projectChannelMessage`. | Append-only historical run ledger once `status` enters terminal state (`SUCCEEDED`, `FAILED`, `CANCELED`). |
| **`DatSchemaDriftEvent`** | `id: UUID`, `pipelineId: UUID`, `syncRunId: UUID`, `streamName: string`, `changeType: enum(COLUMN_ADDED, COLUMN_REMOVED, TYPE_CHANGED, PRIMARY_KEY_ALTERED)`, `diffPayloadJson: object`, `ContainsPotentialPii: boolean`, `resolutionStatus: enum(AUTO_APPLIED, PENDING_STEWARD, APPROVED, BLOCKED, HASHED)`, `resolvedByUserId: UUID?`, `activeWorkItemId: UUID?`, `version: int` | `pipelineId -> DatSyncPipeline.id`; `syncRunId -> DatSyncRun.id`; `activeWorkItemId -> Platform WorkItem.id`. | Any `TYPE_CHANGED` narrowing or `ContainsPotentialPii === true` column addition forces `PENDING_STEWARD` regardless of `AUTO_PROPAGATE_NEW` until reviewed. |
| **`DatTransformationModel`** | `id: UUID`, `code: string`, `name: string`, `modelType: enum(SQL_DBT, VISUAL_PREP)`, `materialization: enum(VIEW, TABLE, INCREMENTAL)`, `upstreamPipelineIds: UUID[]`, `upstreamModelIds: UUID[]`, `sqlOrPrepSpec: string`, `qualityTestsJson: {type, column, config}[]`, `gitRepositoryId: UUID?`, `gitCommitSha: string?`, `targetWarehouseTableId: UUID?`, `lastRunStatus: enum(NEVER_RUN, PASSED, FAILED_QUALITY, ERROR)`, `version: int` | `upstreamPipelineIds -> DatSyncPipeline.id[]`; `targetWarehouseTableId -> DatWarehouseTable.id` (`#099`); `gitRepositoryId -> DevRepository.id` (`#101`). | Dependency graph across `upstreamModelIds` MUST be a strict DAG (cycle check enforced on save). |

---

## 5. Core End-to-End User Journeys (5 Journeys)

1. **Journey 1 — Vault-Backed CDC Pipeline Creation & Preflight Verification:**
   - A Data Engineer opens `source-connections`, clicks `"New Source Connector"` (`connector-pipeline-drawer`), selects `POSTGRES`, binds `vaultSecretRef: "sec://prod-erp-pg-ro"`, and clicks `"Test Preflight"`.
   - Preflight confirms TLS 1.3, `wal_level = logical`, and replication slot health (`HEALTHY`).
   - Creating a `LOG_BASED_CDC` `DatSyncPipeline` targeting `#099` `RAW_ERP`, the engineer enables `orders` and `customers`, checks `hashedColumns: ['tax_id', 'phone']`, saves (drawer closes via `closePageDrawer()`), and triggers an initial sync landing clean tables into `#099`.
2. **Journey 2 — Upstream Schema Drift Quarantine & Steward Resolution via Home `WorkItem`:**
   - Upstream ERP adds column `customer_ssn_last4` to `customers`. During `DatSyncRun`, the PII heuristic flags `ContainsPotentialPii: true`, pauses the `customers` stream (`PENDING_STEWARD`), creates a `DatSchemaDriftEvent`, and dispatches a Rapider Home `WorkItem`.
   - Clicking from the Home `WorkItem` into `schema-drift-center`, the Data Steward selects `"Propagate with Salted SHA-256 Hash"`, updating `DatSyncPipeline.streamsConfigJson`, resolving the `WorkItem`, and automatically resuming the paused stream from its exact `DatReplicationCheckpoint`.
3. **Journey 3 — Post-Load Transformation DAG, Quality Quarantine & Lineage Emission:**
   - Immediately after `DatSyncPipeline` completes, `transformation-models` runs `mart_customer_orders` (`INCREMENTAL` SQL model) on `#099` compute.
   - An inline `NOT_NULL(order_currency)` test catches 3 malformed rows, routes them to dead-letter quarantine (`PARTIAL_QUARANTINE`), emits `dat.governance.quality_incident.created.v1` to `#100`, publishes column-level `DatLineageEdge`s, and triggers `#096` `bi-dashboards` cache refresh for all valid rows.
4. **Journey 4 — Click-Through Sync Run Diagnostics & State-Preserving Back Navigation:**
   - On `sync-runs`, an On-Call Engineer filters by `Status: "FAILED"` (`#activeFilterTagsBar` displays `Status: "FAILED" ✕`) and clicks run identifier `RUN-2026-0891` to open `#/run-attempt-detail?id=...`.
   - The AI Fix Assistant inside `projectChannelMessage` identifies `HTTP 401 OAuth Refresh Token Revoked` on `DatSourceConnection` and proposes rotating the vault secret reference; clicking `"Back to Sync Runs"` restores `sync-runs` with `Status: "FAILED"` preserved.
5. **Journey 5 — Reverse ETL Enrichment Back to `CRM`:**
   - A RevOps Analyst configures a `REVERSE_ETL_SUITE` `DatSyncPipeline` reading `#099` `mart_account_usage_30d` and mapping `active_seats` and `last_login_at` to canonical `CRM` `CrmAccount` enrichment via `crm.account.enrichTelemetry` guarded command with full idempotency and audit logging.

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### `P0` Foundation Work Packages
- **`WP-P0-1` Vault Connectors, Destinations & Preflight (`ETL-001`, `ETL-002`, `ETL-003`):** Build `source-connections`, `destination-targets`, and `connector-pipeline-drawer` with secret-vault binding and preflight health diagnostics (`Data Engineer`).
- **`WP-P0-2` Multi-Mode Sync, CDC Checkpoints, SCD2 & PII Hashing (`ETL-007`, `ETL-008`, `ETL-009`, `ETL-010`, `ETL-011`):** Build `sync-pipelines`, `pipeline-detail`, and `schema-drift-center` supporting full/incremental/CDC modes, SCD2 history, salted hashing, and drift governance (`Data Engineer` / `Data Steward`).
- **`WP-P0-3` Visual Prep & Post-Load SQL Transformations (`ETL-013`, `ETL-014`):** Build `transformation-models` with DAG cycle validation and SME no-code cleansing recipes (`Analytics Engineer` / `Analyst`).
- **`WP-P0-4` Scheduling, Run Ledger & Failure `WorkItem`s (`ETL-019`, `ETL-020`, `ETL-021`):** Build `integration-hub`, `sync-runs`, and `run-attempt-detail` with cron/dependency scheduling, exponential retries, and failure escalation (`Data Engineer`).

### `P1` Scale & Enterprise Governance Work Packages
- **`WP-P1-1` Custom REST/GraphQL Builder, Reverse ETL & Backfills (`ETL-004`, `ETL-005`, `ETL-012`):** Build `custom-connector-studio`, Reverse ETL guarded command writers, and surgical table backfills (`Data Engineer` / `RevOps`).
- **`WP-P1-2` Inline Quality Gates, DAG Visualizer & Git CI Integration (`ETL-015`, `ETL-016`, `ETL-017`):** Dead-letter row quarantine + `#100` incident emission, `dag-lineage-explorer`, and `#101`/`#102` PR dry-run validation (`Analytics Engineer`).
- **`WP-P1-3` Freshness SLAs, MAR Caps, OAuth Warnings & Env Promotion (`ETL-022`, `ETL-023`, `ETL-024`, `ETL-025`):** Downstream `#096` BI stale circuit breakers, `Decimal128` MAR budget caps, 14-day OAuth expiry `WorkItem`s, and `DEV -> PROD` promotion (`Platform Admin`).

### `P2` Adjacent & Grounded AI Work Packages
- **`WP-P2-1` OpenAPI Connector Generator & Document Chunking (`ETL-006`, `ETL-018`):** AI OpenAPI spec parser in `custom-connector-studio` and unstructured Drive document chunking pipelines (`Data Engineer`).
- **`WP-P2-2` AI Field Mapper, Failure Fix Assistant & Postmortem Threads (`ETL-026`, `ETL-027`, `ETL-028`):** Smart schema mapping recommendations, grounded log root-cause diagnostics, and `projectChannelMessage` incident threads (`Data Engineer` / `Analyst`).

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **`Pipeline Viewer`** | View `integration-hub`, `sync-pipelines`, `sync-runs`, and `dag-lineage-explorer` status and freshness SLAs. | Cannot trigger manual resyncs, inspect quarantined row payloads, or view `vaultSecretRef` bindings. |
| **`Business Data Analyst`** | Build visual prep recipes (`VISUAL_PREP`) in `transformation-models`, view non-PII `schema-drift-center` diffs. | Cannot create production `LOG_BASED_CDC` sources, disable PII hashing, or alter `monthlyMarCap`. |
| **`Analytics Engineer`** | Author/edit SQL & visual `DatTransformationModel`s, configure inline quality assertions, link `#101` Git repos, trigger model runs. | Cannot rotate `DatSourceConnection` vault secrets or override `ContainsPotentialPii` quarantine blocks. |
| **`Data Engineer`** | Create/edit `DatSourceConnection`, `DatDestinationTarget`, `DatSyncPipeline`, `custom-connector-studio` specs; trigger surgical backfills and retries. | Cannot read raw secret values from Platform Vault (write/bind reference IDs only); cannot approve PII un-hashing without `Data Steward`. |
| **`Data Steward / Integration Admin`** | Resolve `PENDING_STEWARD` `DatSchemaDriftEvent`s via Home `WorkItem`s, configure PII hashing/blocking rules, set workspace `monthlyMarCap` budgets. | All drift approvals and state resets require guarded command audit justification. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### Guarded Commands
1. `dat.etl.testSourcePreflight({ sourceConnectionId, idempotencyKey, correlationId })` — Runs read-only connectivity, TLS, and CDC log permission checks via isolated worker using `vaultSecretRef`.
2. `dat.etl.triggerSyncRun({ pipelineId, triggerType, selectedStreams, backfillRange, idempotencyKey, correlationId })` — Acquires pipeline concurrency lease and enqueues an idempotent `DatSyncRun`.
3. `dat.etl.commitStreamCheckpoint({ pipelineId, streamName, checkpointStateJson, expectedVersion, idempotencyKey, correlationId })` — Atomically advances `DatReplicationCheckpoint` after staging verification.
4. `dat.etl.resolveSchemaDrift({ driftEventId, resolutionAction, expectedVersion, idempotencyKey, correlationId })` — Applies `APPROVED`, `BLOCKED`, or `HASHED` column policy, closes the Home `WorkItem`, and resumes paused streams.
5. `dat.etl.executeTransformationDag({ rootModelIds, gitCommitSha, idempotencyKey, correlationId })` — Executes topological sort of `DatTransformationModel`s in `#099` and emits lineage + quality telemetry.

### Domain Events Published & Consumed (Kafka)
- **Published:** `dat.etl.sync_run.succeeded.v1`, `dat.etl.sync_run.failed.v1`, `dat.etl.schema_drift.detected.v1`, `dat.etl.mart.refreshed.v1`, `dat.etl.freshness_sla.breached.v1`, `dat.etl.lineage.emitted.v1`.
- **Consumed:** `dev.scm.pull_request.opened.v1` (from `#101` for transformation SQL dry-run previews), `dat.governance.classification.updated.v1` (from `#100` to enforce upstream column hashing).

### `requiredWorkflows`
- `etlScheduledSyncOrchestratorWorkflow` — Cron/lease-guarded extraction, chunked staging load, checkpoint commit, and retry orchestration.
- `etlPostLoadTransformDagWorkflow` — Chains `DatTransformationModel` execution immediately following upstream sync completion.
- `etlSchemaDriftEscalationWorkflow` — Routes breaking or PII-flagged schema changes to Steward Home `WorkItem`s.
- `etlOAuthExpiryMonitorWorkflow` — Daily check creating preventative Home `WorkItem`s 14 days before credential/certificate expiration.

### Grounded Rapider AI Governance Rules
- AI connector builders (`ETL-006`), field mappers (`ETL-026`), and failure assistants (`ETL-027`) NEVER receive raw vault secrets or unmasked PII rows in prompt context, and NEVER execute stream state resets, backfill overwrites, or schema drift approvals without explicit human confirmation.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **Strict Zero-Secret Persistence:** In compliance with Rapider repository security mandates, `DatSourceConnection` and `DatDestinationTarget` reject any payload containing inline passwords, API keys, JDBC passwords, or bearer tokens (`422 Validation Error`); workers resolve `vaultSecretRef` strictly in memory via environment/platform vault.
- **In-Flight PII Protection (GDPR / HIPAA / SOC 2):** Columns configured in `hashedColumns` are hashed with workspace-specific KMS-managed salt before writing to disk or staging buckets; `blockedColumns` are dropped in memory at the extraction reader.
- **Throughput, Idempotency & Precision:** Supports checkpointed multi-million-row incremental loads without memory exhaustion; `Decimal128` preserved across financial ERP extractions (e.g., `\$10,450,000.99` ledger balances) and `monthlyMarCap` quotas; zero `projectId` fields on models.
- **Accessibility & Responsive UX:** WCAG 2.2 AA accessible tabular DAG fallback alongside `dag-lineage-explorer`, `#ffffff !important` white text on primary/danger buttons, and responsive views across `390px`, `768px`, and `1440px`.

---

## 10. Deterministic Acceptance Criteria (14 Testable Criteria)

1. **AC-01 (Sidebar Minimum-3 Rule):** Visible sidebar sections (`Pipelines & Runs`: 3 items, `Connectivity`: 3 items, `Transform & Governance`: 3 items) each contain `>= 3` `displayOnMenu: true` pages; `pipeline-detail`, `run-attempt-detail`, and `connector-pipeline-drawer` set `displayOnMenu: false`.
2. **AC-02 (Zero-Secret Guardrail):** Submitting a `DatSourceConnection` or `DatDestinationTarget` containing a plaintext password, `Bearer ...` token, or credentialed URI (`postgres://user:pass@...`) is deterministically rejected with a `422` validation error; valid `vaultSecretRef` references save cleanly.
3. **AC-03 (SWR Clean Queries & Defensive Filtering):** All 9 directory/hub routes fetch initial data via `findSWR`/`countSWR` with zero `isDeleted` where-clauses and apply in-memory `item.isDeleted !== true` filtering.
4. **AC-04 (Table Actions & Click-Through Navigation):** Zero `"View"` buttons exist in `Actions` columns; clicking a pipeline name on `sync-pipelines` navigates to `#/pipeline-detail?id=...` and clicking a run ID on `sync-runs` navigates to `#/run-attempt-detail?id=...` without `<base href>` 404 traps; `Edit` uses icon+text and `Delete` uses an icon-only button with `#rapiderConfirmOkBtn` modal confirmation.
5. **AC-05 (Active Filter Tags & Clear All):** Selecting `Status: "FAILED"` or typing a search term on `sync-runs` or `schema-drift-center` displays `#activeFilterTagsBar` with HTML-escaped dismissible chips and `[Clear all]`, synchronously updating rows and counters.
6. **AC-06 (Drawer Lifecycle & `finally` Button Reset):** Creating or editing a connector or pipeline in `connector-pipeline-drawer` restores the save button state inside `finally`, invalidates SWR caches, updates parent tables optimistically, and closes the drawer via `closePageDrawer()`.
7. **AC-07 (Pipeline Concurrency Lease & Idempotency):** Invoking `dat.etl.triggerSyncRun` while a `RUNNING` `DatSyncRun` already holds the lease on `pipelineId` queues or rejects duplicate overlap deterministically without corrupting `DatReplicationCheckpoint`.
8. **AC-08 (Atomic Checkpoint Resume):** Simulating a mid-run network failure after chunk 3 of 5 leaves `DatReplicationCheckpoint` at chunk 3; the subsequent retry resumes from chunk 4 without duplicating or dropping rows.
9. **AC-09 (PII Schema Drift Quarantine `WorkItem`):** When an upstream sync introduces a new column matching PII heuristics (`ssn`, `email`, `iban`), `DatSchemaDriftEvent` enters `PENDING_STEWARD`, pauses that stream, and creates a Rapider Home `WorkItem`.
10. **AC-10 (Transformation DAG Cycle Rejection):** Saving a `DatTransformationModel` whose `upstreamModelIds` form a cycle (`A -> B -> A`) fails deterministic validation before persistence.
11. **AC-11 (`Decimal128` MAR Cap & Financial Fidelity):** Currency columns cleansed via `DatTransformationModel` and `monthlyMarCap` comparisons preserve exact `Decimal128` precision.
12. **AC-12 (Downstream BI Stale Circuit Breaker):** When a `DatSyncPipeline` exceeds `freshnessSlaMinutes`, `dat.etl.freshness_sla.breached.v1` is emitted so `#096` `bi-dashboards` flags dependent scorecards as stale.
13. **AC-13 (Back Navigation Filter Preservation):** Navigating from `sync-runs` (filtered by `Status: "FAILED"`) into `run-attempt-detail` and clicking `#btnGoBack` restores `sync-runs` with `Status: "FAILED"` active.
14. **AC-14 (Theme & Contrast Compliance):** All 12 pages pass light/dark `validate_theme` checks with `#ffffff !important` text on primary/destructive buttons and zero `window.alert`/`confirm`/`prompt` dialogs.
