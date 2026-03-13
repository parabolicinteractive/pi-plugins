# Operations & Deployment Guide Template

```markdown
---
title: "[Project Name] — Operations Guide"
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
tags: [operations, deployment, runbook]
---

# Operations Guide

## 1. Environment Overview

### Environments

| Environment | Purpose | URL | Access |
|------------|---------|-----|--------|
| Development | Local development | localhost:NNNN | All developers |
| Staging | Pre-production testing | | Team |
| Production | Live users | | Restricted |

### Infrastructure Components

| Component | Provider | Purpose | Dashboard |
|-----------|----------|---------|-----------|
| | | | |

## 2. Deployment

### Deployment Pipeline

Describe each stage of the CI/CD pipeline:

1. **Build**: What triggers a build. What it produces.
2. **Test**: What tests run. Pass/fail criteria.
3. **Deploy to staging**: Automatic or manual. Approval gates.
4. **Deploy to production**: Process, timing, approval requirements.

### Deployment Checklist

Pre-deployment:
- [ ] All tests passing on the target branch.
- [ ] Database migrations reviewed and tested.
- [ ] Feature flags configured.
- [ ] Rollback plan documented for this release.

Post-deployment:
- [ ] Smoke tests passing.
- [ ] Key metrics within normal range.
- [ ] No new error spikes in monitoring.

### Rollback Procedure

Step-by-step instructions for reverting a deployment:

1. [Step]
2. [Step]
3. [Verification that rollback succeeded]

Time estimate: [how long a rollback takes].

## 3. Monitoring & Alerting

### Key Metrics

| Metric | Normal Range | Warning Threshold | Critical Threshold |
|--------|-------------|-------------------|-------------------|
| | | | |

### Dashboards

| Dashboard | URL | What It Shows |
|-----------|-----|--------------|
| | | |

### Alert Routing

| Severity | Response Time | Channel | Escalation |
|----------|--------------|---------|-----------|
| Critical | < 15 min | | |
| Warning | < 1 hour | | |
| Info | Next business day | | |

## 4. Incident Response

### Severity Levels

| Level | Definition | Examples |
|-------|-----------|----------|
| SEV-1 | Complete outage | |
| SEV-2 | Major degradation | |
| SEV-3 | Minor issue | |

### Incident Procedure

1. **Detect**: How incidents are detected (alerts, user reports).
2. **Triage**: Who responds. How severity is assessed.
3. **Communicate**: Who to notify. Status page updates.
4. **Mitigate**: Immediate actions to reduce impact.
5. **Resolve**: Root cause fix.
6. **Review**: Post-incident review process. Blameless retrospective.

### Common Runbooks

#### [Scenario Name]

**Symptoms**: What you'll see.
**Cause**: Most likely root cause.
**Resolution**:
1. [Step]
2. [Step]
3. [Verification]

<!-- Add a runbook section for each known failure mode. -->

## 5. Database Operations

### Backup & Recovery

- Backup schedule and retention policy.
- Recovery procedure and estimated time.
- Point-in-time recovery capability.

### Migration Process

- How migrations are created, reviewed, and applied.
- Zero-downtime migration requirements.
- Rollback procedure for failed migrations.

## 6. Scaling

### Auto-Scaling Rules

| Component | Min | Max | Scale-Up Trigger | Scale-Down Trigger |
|-----------|-----|-----|-----------------|-------------------|
| | | | | |

### Manual Scaling Procedures

When and how to manually scale components for anticipated load (events, launches).

## 7. Security Operations

### Access Management

- Who has access to what. How access is granted/revoked.
- Key rotation schedule.
- Secret management process.

### Security Monitoring

- What's being monitored for security events.
- Response procedure for security incidents.

## 8. Maintenance Windows

- Scheduled maintenance timing and communication process.
- What can and cannot be done during maintenance.

## Appendices

- Related documents: [Architecture](architecture.md), [Requirements](requirements.md).
- Vendor documentation links.
- Contact list for escalation.
```
