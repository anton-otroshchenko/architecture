# Error Logging Service

## SDK
**Technology:** Lightweight JavaScript/TypeScript SDK

**Responsibilities:**
- Capture errors automatically and through manual API calls.
- Collect context (environment, user, tags).
- Batch and send logs efficiently with retry logic.

**Reasoning:** Small footprint, works in browser and Node environments, and simple integration.

---

## Backend API
**Technology:** Node.js (Express or Fastify).

**Responsibilities:**
- Ingest error reports from SDKs.
- Validate and normalize payloads.
- Forward structured data to the database and large payloads to object storage.
- Provide APIs for the dashboard.
- Sends alerts via email to the user when critical error occurs, we can utilize SendGrid for this task

**Reasoning:** Node.js supports high concurrency and lightweight request handling, making it ideal for real-time log ingestion.

---

## Database
**Technology:** PostgreSQL (AWS RDS).

**Responsibilities:**
- Store structured metadata for errors (timestamp, project, user, tags).
- Support JSONB columns for flexible storage of variable error payloads.
- Enable indexing and advanced queries for filtering and reporting.

**Reasoning:**
- PostgreSQL provides strong consistency and relational modeling for structured metadata.
- JSONB fields allow storing dynamic error details without strict schemas.
- Advanced indexing enables fast searches across projects, tags, and error messages.
- Managed RDS setup ensures backups, replication, and automatic failover.

---

## Web Dashboard
**Technology:** Next.js + TypeScript.

**Responsibilities:**
- Authentication and project management.
- Browse and search errors with filters.
- View detailed error information (stack trace, metadata).

**Reasoning:** Next.js ensures fast loads and SEO for login/landing pages and supports maintainable UI development.

---

## Deployment & DevOps
**Platform:** AWS.

**Components:**
- **Elastic Beanstalk** → Deploy and scale Node.js API and Next.js frontend.
- **RDS (PostgreSQL)** → Managed DB with backups and high availability.
- **S3** → Store raw error payloads and attachments.
- **CloudWatch** → Monitor metrics, logs, and set alerts.

**Reasoning:** Managed AWS services reduce operational overhead while ensuring scalability, reliability, and security.

---

## Benefits
- Lightweight SDK keeps client apps fast.
- Node.js backend efficiently handles requests.
- PostgreSQL balances structured data with flexible error payloads.
- Next.js dashboard delivers a modern, responsive UI.
- AWS infrastructure ensures scalability, monitoring, and resilience.  

## Key Decisions

### Real-Time Processing
- The API processes and stores logs as they arrive.
- WebSocket or SSE can be added for live updates in the dashboard.

### Log Retention Policies
- Short-term storage (e.g., 30–90 days) in PostgreSQL for fast querying.
- Long-term archiving in S3 with lifecycle policies for cost efficiency.

### API Rate Limits
- Prevent SDK misuse or excessive traffic by applying per-project rate limits.

### Security
- SDK authentication via API keys.

### DevOps Processes
- CI/CD pipelines for SDK, API, and dashboard.
- Automated tests before deployment.
- Infrastructure monitoring via CloudWatch.
- Rollback strategies in case of failed releases. 

## Questions for the Client

1. What kinds of errors should the SDK capture (runtime exceptions, network errors, custom logs, performance metrics)?
2. Should there be a set of errors/features available only to paid users, or is the SDK itself only for paid customers?
3. Do you require real-time updates in the dashboard (using Server-Sent Events or WebSockets), or is periodic refresh sufficient?
4. What is the expected log retention policy (e.g., 30 days, 90 days, custom per plan)?
5. Should the system support alerts beyond email (e.g., Slack, Microsoft Teams, SMS)?  
