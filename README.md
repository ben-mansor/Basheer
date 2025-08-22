Goal: Design and implement a secure, scalable Archive System that supports data entry, advanced search, and reporting, with strong governance, auditability, and lifecycle management.

0) Context & Constraints

Organization: {ORG_NAME}

Primary Content Types: {DOC_TYPES} e.g., contracts, invoices, case files, lab results, photos

Volume: {EST_DOCS}/year, average size {AVG_SIZE}, peaks {PEAK_RATE}/min

Compliance: {COMPLIANCE} e.g., ISO 27001, GDPR, HIPAA, SOC 2

Hosting: {CLOUD_OR_ON_PREM}

Languages: {LANGS}, default {DEFAULT_LANG}

Budget/SLA targets: RPO {RPO}, RTO {RTO}, uptime {SLA}%, P95 search latency < {SEARCH_P95} ms

1) Users, Roles, Permissions

Roles: Admin, Archivist, Reviewer/Approver, Contributor, Read-only, Auditor, External/Guest.

RBAC with resource-level ACLs; support org/department/project scoping.

Fine-grained rights: create/read/update/delete, bulk actions, export, schema edits, retention overrides, legal hold, report design, API access.

SSO/SAML/OIDC, SCIM user provisioning, MFA.

2) Data Model & Metadata

Core entities: Document, Folder/Collection, Record (structured entry), Person/Organization, Case/Project.

Metadata schema: versioned, admin-editable; field types (string, number, enum, date, datetime, geo, currency, boolean, array, JSON), required/optional, regex/validation, default values.

Taxonomy & tags: hierarchical categories, synonyms/aliases, controlled vocabularies.

Relationships: one-to-many (case→documents), many-to-many (people↔cases), with typed edges.

Versioning: immutable file versions, diffable metadata, check-in/out, change comments.

Deduplication: content hashing, similarity (perceptual hash for images/PDFs), merge/split flows.

3) Ingestion & Data Entry

Manual forms: form builder tied to metadata schema (conditional fields, repeatable sections).

Bulk import: CSV/JSON/Parquet; watch folders; S3/GCS buckets; email ingestion (inbox@archive.{ORG}); API upload; EDI where applicable.

Scans & OCR: automatic OCR for PDFs/images; multi-language; extract text + layout; confidence scores.

Entity extraction: NER for names, dates, IDs; auto-suggest tags; pluggable extraction pipelines.

Validation & QC: preflight checks, field validation, duplicate detection, required attachments.

Workflows: draft → review → approved → archived; configurable states, SLAs, and parallel approvals.

Templates: reusable field sets per document type.

4) Storage & Lifecycle

Object storage for binaries (immutable), database for metadata.

Retention policies: per type/status; auto disposition (retain, delete, transfer), legal holds, retention exceptions.

Cold storage tiers: move infrequently accessed data; rehydration flow.

Backups & DR: encrypted backups, cross-region replication, restore drills.

5) Search & Discovery

Indexing: full-text (including OCR text), fielded metadata, n-grams, phonetic, synonyms, stemming; numeric/date/geo indexing; incremental + bulk reindex.

Query features: keyword, boolean, phrase, fuzzy, proximity, wildcard, filter facets, date ranges, geo-radius/polygons, “more like this”.

UI facets: by type, tag, owner, status, date, department, retention class, confidence level.

Relevance: BM25 + boosts by field/type/recency; typo tolerance; query suggestions & did-you-mean.

Saved searches & alerts: users can save and subscribe; export results.

Security filtering: results constrained by permissions at query time.

6) Reporting & Analytics

Report builder: drag-and-drop fields, computed columns, filters, groupings, pivots, charts (bar/line/pie/stacked), aggregations (sum/avg/min/max/count/distinct), date bucketing.

Templates: scheduled reports (daily/weekly/monthly), parameterized (e.g., date range).

Exports: CSV, XLSX, PDF; API retrieval; row-level security enforced.

Operational dashboards: ingestion throughput, backlog, SLA breaches, OCR success, storage usage.

Data catalog lineage: show source → transformations → index/report dependencies.

7) Security & Compliance

Encryption: at rest (KMS) and in transit; optional client-side encryption for sensitive classes.

Secrets management: vault integration.

PII/PHI handling: field-level masking, purpose limitation flags, consent tracking.

Audit logging: immutable logs for auth, reads/writes, exports, admin actions; tamper-evident (hash-chain).

DLP: pattern detectors (SSN, IBAN), quarantine workflows, redaction tools.

eDiscovery: legal hold, case exports with manifest, Bates numbering.

Compliance reports: access reviews, retention attestations, audit exports.

8) Admin & Operations

Schema editor: add/edit fields, enums, validations, forms, and workflows with preview & versioning.

Config mgmt: per-environment configs, feature flags, migrations.

Observability: metrics, traces, logs; health endpoints; alerting (Slack/Email/Webhook).

Scaling: horizontal scale for API, workers, and search cluster; back-pressure; retry & DLQ.

Job orchestration: ingestion/OCR/indexing/report schedules with progress & retries.

Localization: i18n/l10n for UI, date/number formats; RTL support as needed.

Accessibility: WCAG 2.2 AA.

9) APIs & Integrations

APIs: REST + GraphQL for CRUD, search, reports, admin; auth via OAuth2 client credentials & JWT.

Webhooks/Event bus: document.created/updated/indexed, workflow.state_changed, retention.applied, report.generated.

Connectors: SharePoint/Drive/Dropbox/Box, Email, SFTP; RDBMS snapshot imports; ticketing (Jira/ServiceNow).

SDKs: TS/JS, Python. Example code + Postman collection/OpenAPI spec.

10) UI/UX Requirements

Global search bar with advanced query dialog.

Record view: preview pane (PDF/images/audio/video), metadata tabs, versions, relations graph.

Bulk actions: tag, move, export, change status, assign reviewers.

Keyboard shortcuts, quick actions, undo for safe ops.

Report designer with live preview; shareable links with scoped permissions.

Mobile-responsive layouts; offline draft capture optional.

11) Non-Functional Requirements

Performance: P95 search < {SEARCH_P95} ms for result sets ≤ {RESULTS_SIZE}, P95 upload < {UPLOAD_P95} s for {FILE_SIZE}.

Concurrency: {CONCURRENT_USERS} active users.

Data limits: single doc up to {MAX_FILE_SIZE}, archive total {TOTAL_CAPACITY}.

Availability: {SLA}% with zero-data-loss within {RPO}.

Cost: provide capacity plan and cost estimates per tier (hot/cold).

12) Data Quality & Governance

Mandatory metadata rules; completeness scores.

Duplicate & conflict resolution policies.

Controlled vocab lifecycle (propose → approve → deprecate).

Quarterly access reviews; automated least-privilege suggestions.

13) Testing & Acceptance

Unit/integration/e2e tests; test data generators; red-team scenarios.

Performance/load tests with target SLAs.

Security tests: SAST/DAST, dependency scanning, pentest checklist.

Acceptance criteria examples:

OCR’d PDF becomes searchable within {INDEX_SLA} minutes.

Saved search with filters returns identical results after reindex.

Report “Monthly Invoices by Department” matches baseline dataset within 0.1% variance.

Legal hold prevents deletion via UI and API; logged accordingly.

14) Migration & Rollout

Data migration plan: mapping, field transforms, file moves, cutover.

Pilot with {PILOT_DEPT}; success metrics; training materials; knowledge base.

Rollback strategy; feature flags for risky components.

15) Nice-to-Have (if feasible)

Semantic search (dense vectors), RAG over archives, “ask your archive” chat with citations.

Image/video understanding: object detection, speech-to-text for AV.

Automated redaction (text & image regions).

Timeline/graph visualization of relationships.

Offline desktop capture tool with sync.

16) Deliverables

Architecture diagram(s); sequence diagrams for ingestion & search.

ERD & index design; retention matrix; workflow definitions.

OpenAPI/GraphQL schema; SDKs; admin runbook; user guides.

IaC scripts; CI/CD pipelines; infra dashboards.

Demo dataset and scripted demo path.

Output: Provide:

A detailed system architecture; 2) prioritized roadmap (MVP → v1 → v2); 3) wireframes for key screens; 4) API/OpenAPI spec stubs; 5) test plan; 6) deployment plan; 7) risks & mitigations; 8) cost model; 9) security & compliance checklist.

MVP scope (must-have):

Manual & bulk ingestion (CSV/PDF) with OCR.

Versioned metadata + tagging + relations.

Full-text + faceted search with saved searches.

Role-based access, audit logs, retention policies (basic).

Report builder (filters, aggregations, export CSV/PDF).

Backups, monitoring, and basic dashboards.

Prioritize clarity, sensible defaults, and admin self-service. Provide concrete examples, sample payloads, and acceptance tests for each feature.
