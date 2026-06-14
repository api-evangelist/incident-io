# incident.io GraphQL Schema

This directory contains a conceptual GraphQL schema for the [incident.io](https://incident.io) incident management platform. The schema is derived from the public REST API documented at [https://api-docs.incident.io/](https://api-docs.incident.io/) and reflects the platform's core resource model.

## Source

- REST API docs: https://api-docs.incident.io/
- GitHub org: https://github.com/incident-io
- Schema file: [incident-io-schema.graphql](incident-io-schema.graphql)

## Coverage

The schema defines **60+ named types** organized into the following domains:

### Incident Lifecycle
- `Incident` — core incident resource with all related associations
- `IncidentDetails` — computed view including resolution times and impact
- `IncidentStatus` — status definitions with lifecycle category (Triage, Active, Monitoring, Closed)
- `IncidentSeverity` — severity levels (SEV0–SEV4) with rank ordering
- `IncidentType` — incident categorization (Outage, Security, Data Loss, etc.)
- `IncidentTimestamps` — structured timestamps (reported, acknowledged, resolved, closed)
- `CustomTimestamp` — user-defined named timestamps
- `IncidentUpdate` — status update posts with actor and changed fields
- `IncidentUpdateAction` — summary of what changed in each update

### Roles & Membership
- `IncidentRole` — role definitions (Incident Lead, Comms Lead, etc.)
- `IncidentRoleAssignment` — role-to-user binding per incident
- `IncidentMembership` — users who have joined an incident

### Custom Fields & Metadata
- `CustomField` — field definitions supporting text, numeric, select, user, and catalog entry types
- `CustomFieldOption` — options for SELECT/MULTI_SELECT fields
- `CustomFieldValue` — field values scoped to a specific incident

### Attachments & Links
- `IncidentAttachment` — files and URLs attached to incidents
- `AttachmentResource` — the provider resource behind an attachment
- `IncidentLink` — typed links to related incidents and documents
- `ExternalIssueReference` — references to Jira, Linear, GitHub Issues, etc.

### Follow-Ups & Actions
- `FollowUp` — post-incident action items with status and assignee
- `FollowUpAssignee` — user assigned to a follow-up
- `Action` — live action items tracked during an active incident
- `ActionAssignee` — user assigned to an action

### Timeline
- `Timeline` — full ordered event log for an incident
- `TimelineEntry` — individual event with timestamp, description, and source
- `TimelineSource` — origin of a timeline event (Slack, webhook, manual, etc.)

### Users
- `User` — organisation member with Slack identity
- `UserDetails` — extended profile with on-call and escalation links
- `UserRole` — permission role within the organisation

### On-Call & Escalations
- `EscalationPath` — named escalation policy with ordered nodes
- `EscalationPathNode` — level, schedule, channel, or repeat node
- `EscalationLevel` — group of targets with acknowledgement timeout
- `EscalationTarget` — individual target (user, schedule, or Slack channel)
- `EscalationRepeatConfig` — repeat count for cycling escalation paths
- `Schedule` — on-call schedule with rotations and current on-call users
- `ScheduleRotation` — rotation definition within a schedule
- `OnCallUser` — user currently on-call with shift window

### Integrations
- `Slack` — Slack workspace integration with channel list
- `SlackChannel` — individual Slack channel
- `Pagerduty` — PagerDuty integration with service list
- `PagerdutyService` — a PagerDuty service
- `Opsgenie` — OpsGenie integration with team list
- `OpsgenieTeam` — an OpsGenie team
- `Integration` — generic integration record for all supported providers
- `AlertSource` — inbound alert source configuration
- `Alert` — individual alert event with incident linkage

### Catalog
- `CatalogType` — catalog schema definition (Service, Team, Environment, etc.)
- `CatalogEntry` — a catalog entry instance with attribute values
- `CatalogAttribute` — attribute schema for a catalog type
- `CatalogAttributeBacklink` — reverse relationship between catalog types
- `CatalogAttributeValue` — resolved value for an attribute on an entry

### Workflows & Automation
- `Workflow` — automation rule triggered by incident events
- `WorkflowTrigger` — trigger definition with type and parameters
- `WorkflowTriggerParameter` — individual trigger parameter binding
- `WorkflowCondition` — conditional guard evaluated before workflow runs
- `WorkflowStep` — an individual automation step
- `WorkflowStepParameterBinding` — parameter binding for a step
- `WorkflowConfig` — organisation-level workflow configuration

### Auth & API Access
- `APIKey` — API key with role assignments and usage metadata
- `Token` — OAuth token with scopes and expiry
- `Webhook` — webhook endpoint subscription with event type filter

### Pagination
- `PageInfo` — cursor-based pagination metadata
- `IncidentConnection` — paginated incident list
- `CatalogEntryConnection` — paginated catalog entry list
- `UserConnection` — paginated user list

## Root Operations

### Query
Full read access across all domains: incidents (with filtering), statuses, severities, types, roles, custom fields, users, follow-ups, actions, timelines, catalog types and entries, escalation paths, schedules, workflows, webhooks, API keys, integrations, and alert sources.

### Mutation
Write operations for the incident lifecycle: create/update/close incidents, assign/unassign roles, post status updates, create/update follow-ups and actions, create/delete webhooks.

### Subscription
Real-time subscriptions for incident updates, new incident creation, and timeline entry additions.

## Enums

| Enum | Values |
|------|--------|
| `IncidentStatusCategory` | TRIAGE, ACTIVE, MONITORING, CLOSED, DECLINED, MERGED |
| `IncidentVisibility` | PUBLIC, PRIVATE |
| `IncidentSeverityLevel` | SEV0, SEV1, SEV2, SEV3, SEV4 |
| `FollowUpStatus` | OUTSTANDING, COMPLETED, DELETED, NOT_DOING |
| `ActionStatus` | OUTSTANDING, IN_PROGRESS, COMPLETED, DELETED, NOT_DOING |
| `CustomFieldFieldType` | TEXT, LINK, NUMERIC, SELECT, MULTI_SELECT, USER, USER_LIST, CATALOG_ENTRY, CATALOG_ENTRY_LIST |
| `WorkflowTriggerType` | INCIDENT_CREATED, INCIDENT_STATUS_CHANGED, INCIDENT_SEVERITY_CHANGED, INCIDENT_UPDATED, INCIDENT_CLOSED, MANUAL |
| `WebhookEventType` | 13 event types covering incident, action, follow-up, catalog, and escalation events |
| `EscalationPathNodeType` | LEVEL, SCHEDULE, NOTIFY_CHANNEL, REPEAT |
| `AlertSourceType` | PAGERDUTY, OPSGENIE, GRAFANA, DATADOG, CLOUDWATCH, PROMETHEUS, SENTRY, NEW_RELIC, CUSTOM |
| `CatalogAttributeType` | TEXT, BOOL, NUMBER, STRING, DATETIME, CATALOG_ENTRY, USER |
| `IntegrationProvider` | 13 providers including Slack, Teams, PagerDuty, Jira, Linear, Datadog, and more |
