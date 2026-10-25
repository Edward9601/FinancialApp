<!--
Sync Impact Report
Version change: unversioned scaffold → 1.0.0
Modified principles: empty template slots → I. Investor Education and User Trust;
II. Advice, Compliance, and Product Boundaries; III. Brokerage Data Privacy,
Consent, and Security; IV. Reliable Data Acquisition and Source Provenance;
V. AI Grounding, Safety, and Explainability; VI. Forecasting Must Be Measurable,
Not Speculative; VII. Angular and .NET Architecture; VIII. Data Integrity and API
Quality; IX. Testing, Evaluation, and Release Quality.
Added sections: Product and Technical Constraints; Development Workflow.
Removed sections: None.
Follow-up TODO: confirm the original ratification date.
-->

# FinancialApp Constitution

## Core Principles

### I. Investor Education and User Trust (NON-NEGOTIABLE)

- The product's primary purpose is to educate users and help them understand
  investments, risk, diversification, fees, and market information.
- Clearly distinguish educational content, general market commentary, portfolio
  observations, forecasts, and any future personalized recommendation features.
- Never promise returns, imply certainty, use misleading performance claims, or
  present forecasts as facts.
- Every forecast and AI-generated investment insight MUST show its assumptions,
  relevant data date, uncertainty, limitations, and a plain-language risk
  disclaimer.
- Use language suitable for non-expert investors and explain technical terms.
- The system MUST abstain or ask for clarification when data is incomplete, stale,
  contradictory, or insufficient for a reliable response.

### II. Advice, Compliance, and Product Boundaries

- The product MUST provide educational insights and analysis, not individualized
  investment advice, unless a compliance-approved advisory model is explicitly
  implemented.
- The product MUST NOT execute trades, rebalance accounts, transfer funds, or
  submit brokerage orders.
- The product MUST NOT generate buy, sell, or hold instructions without explicit
  product, legal, and compliance approval.
- Any feature that may be interpreted as personalized advice MUST be flagged for
  legal and compliance review before implementation.
- Conflicts of interest, affiliations, sponsored content, data-source
  limitations, and model limitations MUST be disclosed prominently and
  accurately.
- Architecture MUST allow jurisdiction-specific compliance requirements to be
  introduced without rewriting core business logic.

### III. Brokerage Data Privacy, Consent, and Security

- Brokerage connections MUST be opt-in, read-only by default, and revocable.
- Request only the minimum account permissions and data necessary for the
  user-approved feature.
- Use secure, supported broker or aggregation-provider authorization flows.
  Never collect, store, or log brokerage passwords.
- Encrypt sensitive data in transit and at rest. Store tokens and credentials in
  a dedicated secrets-management solution, never in source code, logs, browser
  storage, or application databases without appropriate encryption.
- Provide clear consent, connection status, revocation, data-export, and
  account-deletion flows.
- Enforce user-level authorization so a user can access only their own accounts,
  holdings, transactions, goals, and insights.
- Maintain audit trails for brokerage-data access, consent changes, data imports,
  and material AI-generated outputs.

### IV. Reliable Data Acquisition and Source Provenance

- Prefer licensed APIs, official feeds, RSS feeds, or sources that explicitly
  permit programmatic access over web scraping.
- Web scraping MUST respect source terms, robots directives, rate limits,
  copyright, and applicable law.
- Every ingested document or data point MUST include source URL, publisher, title,
  publication date, ingestion date, content hash, license or usage status, and
  retrieval method.
- Preserve raw source files in S3-compatible object storage with encryption,
  restricted access, retention rules, and immutable identifiers.
- Separate raw source content, normalized data, derived data, embeddings, and
  AI-generated summaries.
- AI summaries MUST NOT obscure, replace, or misrepresent their underlying
  sources. Surface citations, source dates, and relevant excerpts whenever AI
  uses external financial information to answer a user.

### V. AI Grounding, Safety, and Explainability

- AI responses about markets, securities, portfolios, or forecasts MUST be
  grounded in retrieved, dated, attributable sources or clearly labeled as
  general educational content.
- The system MUST NOT allow a model to fabricate prices, holdings, performance
  figures, sources, events, or citations.
- Use retrieval with source filtering, recency checks, provenance metadata, and
  permission-aware access controls.
- Treat user prompts, uploaded documents, brokerage data, and retrieved web
  content as untrusted input. Defend against prompt injection and data
  exfiltration.
- Do not send sensitive brokerage or personal data to an AI provider unless that
  use is explicitly authorized, documented, and protected.
- Record the model version, prompt/template version, source set, input-data
  timestamp, response timestamp, and relevant evaluation result for material
  insights and forecasts.
- Explain in user-friendly language why an insight was generated, what data was
  considered, and what the system does not know.

### VI. Forecasting Must Be Measurable, Not Speculative

- A forecast MUST be a time-bound, versioned prediction with a defined target,
  horizon, assumptions, confidence or uncertainty range, and evaluation criteria.
- Store forecasts separately from market data and source documents, with
  immutable timestamps and the model/data versions that produced them.
- Evaluate forecasts only against data available after the forecast was made;
  prevent look-ahead bias and data leakage.
- Track accuracy, calibration, error, coverage, and performance by market
  condition and prediction type.
- Saving forecasts MUST NOT be described as model learning. Model improvements
  require documented evaluation, validated training or tuning data, offline
  testing, review, and controlled deployment.
- Compare new models or forecast approaches against baselines before release.
  Never silently replace a production forecasting method.
- Show prior forecast outcomes, including misses, fairly and understandably.

### VII. Angular and .NET Architecture

- Angular MUST use standalone, accessible components; strongly typed API
  services; route guards; reactive forms; and explicit loading, empty, error, and
  permission-denied states.
- ASP.NET Core MUST maintain clear boundaries between API/endpoints, application
  services, domain logic, data access, and external integrations.
- Keep controllers and endpoints thin. Keep financial rules, authorization
  decisions, data validation, and AI orchestration in testable services.
- Use strongly typed request/response DTOs. Never expose database entities,
  secret fields, brokerage tokens, or internal system details through APIs.
- Use asynchronous I/O, dependency injection, structured logging, correlation
  IDs, health checks, and environment-based configuration.
- Store documents and forecast artifacts in S3-compatible storage. Store only
  metadata, access references, and necessary relational data in the application
  database.

### VIII. Data Integrity and API Quality

- Validate all client and server input, including investment goals, risk
  responses, account imports, dates, amounts, symbols, pagination, and uploaded
  files.
- Use decimal types for money and explicit currency codes. Never use
  floating-point values for currency calculations.
- Make data-import and analysis jobs idempotent, observable, retry-safe, and
  resilient to partial failures.
- Use standardized API errors with safe, actionable messages and correct HTTP
  status codes.
- Paginate and filter all scalable collections. Optimize queries and avoid
  unnecessary sensitive payloads.
- Explicitly identify stale, delayed, missing, or partial market and brokerage
  data in the user experience.

### IX. Testing, Evaluation, and Release Quality

- Unit-test financial calculations, authorization, validation, data
  normalization, forecast evaluation, and business rules.
- Integration-test brokerage-provider boundaries, account isolation, data
  ingestion, object-storage access, API authorization, and critical user
  workflows.
- Test AI behavior for hallucination, prompt injection, unsafe advice, missing
  citations, stale data, misleading confidence, and unauthorized data exposure.
- Each AI feature MUST have documented evaluation cases before release,
  including expected safe refusals and uncertainty handling.
- Builds, linting, formatting, security scanning, and relevant tests MUST pass
  before a feature is complete.
- No production release may expose real brokerage data or personalized
  financial insights without security and compliance review.

## Product and Technical Constraints

- Brokerage integrations are optional, read-only, revocable, and least-privilege
  by default.
- Financial insights and forecasts must preserve their data provenance,
  uncertainty, limitations, and intended educational scope throughout the
  product experience.
- Any exception to this constitution requires a documented rationale, risk
  assessment, and explicit approval.

## Development Workflow

- Begin each feature specification by recording the intended user benefit,
  whether it is educational or advisory, data sources, requested permissions,
  AI behavior, risks, failure states, compliance impact, and acceptance criteria.
- Treat web content and market data as potentially stale, incomplete, biased,
  licensed, or adversarial until verified.
- For each forecast feature, define the prediction target, horizon, baseline,
  evaluation method, and user-facing uncertainty statement before
  implementation.
- Document assumptions. Ask for clarification rather than inventing investment
  rules, risk tolerance, regulatory status, or data rights.

## Governance

- This constitution supersedes conflicting implementation preferences.
- Amendments MUST document their rationale and impact, undergo security and
  compliance review where relevant, and increment the version according to
  semantic versioning: MAJOR for backward-incompatible governance or principle
  changes, MINOR for new principles or materially expanded guidance, and PATCH
  for clarifications or non-semantic refinements.
- Specifications, plans, pull requests, and releases MUST verify compliance with
  applicable principles. Production releases involving brokerage data or
  personalized insights require the reviews in Principle IX.
- The project MUST review this constitution whenever product scope, regulatory
  obligations, or data practices materially change.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm original adoption date | **Last Amended**: 2026-10-25
