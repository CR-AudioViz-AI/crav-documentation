# Monitoring Setup Guide

**Admin Infrastructure Guide**  
**Updated:** November 21, 2025 - 5:03 PM EST

---

## Overview

Complete monitoring stack for production readiness.

---

## Health Checks

### Endpoint Setup
**Create:** `/api/health`

```typescript
export async function GET() {
  const checks = {
    database: await checkDatabase(),
    redis: await checkRedis(),
    apis: await checkAPIs(),
    storage: await checkStorage()
  }
  
  return Response.json({
    status: allHealthy(checks) ? 'healthy' : 'degraded',
    checks,
    timestamp: new Date().toISOString()
  })
}
```

**Uptime Monitoring:**
- Service: UptimeRobot (free)
- Check every: 5 minutes
- Alert: Email + Slack
- Public status: status.craudiovizai.com

---

## Error Tracking

### Vercel Built-in
**Auto-enabled** with deployment
- Runtime errors
- Build failures
- Function timeouts

**Access:** Vercel Dashboard → Logs

### Custom Errors
```typescript
// Log to database
await logError({
  message: error.message,
  stack: error.stack,
  user: userId,
  context: { ...metadata }
})

// Alert if critical
if (isCritical) {
  await sendAlert('slack', error)
}
```

---

## Performance Monitoring

### Web Vitals
**Vercel Analytics** (included)
- LCP (Largest Contentful Paint)
- FID (First Input Delay)
- CLS (Cumulative Layout Shift)

**Targets:**
- LCP: <2.5s
- FID: <100ms
- CLS: <0.1

### API Response Times
**Track in Database:**
```sql
CREATE TABLE api_metrics (
  endpoint TEXT,
  duration_ms INT,
  status_code INT,
  timestamp TIMESTAMPTZ
);
```

**Query slow endpoints:**
```sql
SELECT endpoint, AVG(duration_ms)
FROM api_metrics
WHERE timestamp > NOW() - INTERVAL '1 hour'
GROUP BY endpoint
HAVING AVG(duration_ms) > 1000;
```

---

## Database Monitoring

### Supabase Dashboard
**Built-in metrics:**
- Connection count
- Query performance
- Database size
- Replication lag

**Access:** Supabase → Database → Performance

### Custom Queries
```sql
-- Slow queries
SELECT query, calls, total_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;

-- Table sizes
SELECT tablename, pg_size_pretty(pg_total_relation_size(tablename::text))
FROM pg_tables
WHERE schemaname = 'public';
```

---

## Alerts

### Critical (Immediate)
**Triggers:**
- Database down
- API error rate >5%
- Payment processing failed
- Security breach

**Notify:** Phone + Email + Slack

### High (15 min)
**Triggers:**
- API response time >2s
- Error rate >2%
- Storage >90% full

**Notify:** Slack + Email

### Medium (1 hour)
**Triggers:**
- Backup failed
- Performance degraded
- High resource usage

**Notify:** Slack

### Low (24 hours)
**Triggers:**
- Dependency updates
- Scheduled maintenance
- General notices

**Notify:** Email

---

## Slack Integration

### Setup Webhook
```bash
# Add to Vercel env
SLACK_WEBHOOK_URL=https://hooks.slack.com/...
```

### Send Alerts
```typescript
async function sendSlackAlert(message: string, severity: 'critical' | 'high' | 'medium') {
  const color = {
    critical: '#ff0000',
    high: '#ff9900',
    medium: '#ffff00'
  }[severity]
  
  await fetch(process.env.SLACK_WEBHOOK_URL, {
    method: 'POST',
    body: JSON.stringify({
      attachments: [{
        color,
        title: `${severity.toUpperCase()} Alert`,
        text: message,
        ts: Date.now() / 1000
      }]
    })
  })
}
```

---

## Cost Monitoring

### Vercel Usage
**Dashboard:** Vercel → Usage
- Function invocations
- Bandwidth
- Build minutes

**Alerts:** Email at 80% of limits

### Supabase Usage
**Dashboard:** Supabase → Usage
- Database size
- Storage
- API requests

**Monitor:** Daily review

### AI API Costs
**Track per request:**
```typescript
await trackAICost({
  provider: 'openai',
  model: 'gpt-4',
  tokens: { input: 1000, output: 500 },
  cost: 0.035
})
```

**Query monthly spend:**
```sql
SELECT provider, SUM(cost)
FROM ai_usage
WHERE created_at > DATE_TRUNC('month', NOW())
GROUP BY provider;
```

---

## Logs

### Vercel Logs
```bash
# View real-time
vercel logs --follow

# Filter by function
vercel logs --filter=api/chat

# Specific deployment
vercel logs <deployment-url>
```

### Supabase Logs
**Access:** Dashboard → Logs
- API logs
- Database logs
- Auth logs

**Retention:** 7 days (free), 90 days (paid)

### Custom Logging
```typescript
// Structured logging
console.log(JSON.stringify({
  level: 'info',
  message: 'User action',
  userId,
  action: 'credit_purchase',
  metadata: { amount: 100 }
}))
```

---

## Metrics Dashboard

### Create Admin View
**Show:**
- Active users (24h)
- Credits purchased (24h)
- Tools used (top 10)
- Error rate
- API response times
- System health

**Update:** Real-time via WebSocket

### Key Metrics
```typescript
const metrics = {
  users: {
    active: await countActiveUsers(24), // hours
    new: await countNewUsers(24),
    total: await countTotalUsers()
  },
  revenue: {
    today: await getRevenue(1), // days
    month: await getRevenue(30),
    credits_sold: await getCreditsSold(1)
  },
  system: {
    error_rate: await getErrorRate(1),
    avg_response: await getAvgResponseTime(1),
    uptime: await getUptime(30)
  }
}
```

---

## On-Call Rotation

### Setup
**Tool:** PagerDuty or OpsGenie (optional)
**Schedule:** Weekly rotation
**View:** admin.craudiovizai.com/on-call

### Responsibilities
- Respond to critical alerts
- Investigate issues
- Escalate if needed
- Document incidents

### Contact Methods
- Phone (critical)
- Slack (high/medium)
- Email (low)

---

## Incident Response

### When Alert Fires
1. **Acknowledge** (2 min)
2. **Assess** severity (5 min)
3. **Investigate** root cause (15 min)
4. **Resolve** or escalate (30 min)
5. **Document** in post-mortem

### Create Incident
```typescript
await createIncident({
  title: 'Database connection timeout',
  severity: 'critical',
  started_at: new Date(),
  affected_users: 1200,
  status: 'investigating'
})
```

### Update Status Page
```typescript
await updateStatus({
  component: 'database',
  status: 'degraded',
  message: 'Investigating connection issues'
})
```

---

## Post-Mortem Template

```markdown
# Incident Post-Mortem

**Date:** [date]
**Duration:** [start] - [end]
**Severity:** [critical/high/medium]
**Impact:** [users affected]

## Timeline
- [time]: Issue detected
- [time]: Investigation began
- [time]: Root cause identified
- [time]: Fix deployed
- [time]: Verified resolved

## Root Cause
[What happened]

## Resolution
[How we fixed it]

## Prevention
[Changes to prevent recurrence]

## Action Items
- [ ] [task] - Owner: [name]
- [ ] [task] - Owner: [name]
```

---

## Testing

### Load Testing
**Tool:** k6 or Artillery

```javascript
// k6 script
import http from 'k6/http';

export let options = {
  vus: 100, // virtual users
  duration: '5m'
};

export default function() {
  http.get('https://craudiovizai.com/api/health');
}
```

### Smoke Tests
```bash
# Run after deployment
npm run test:smoke

# Check critical paths
curl https://craudiovizai.com/api/health
curl https://craudiovizai.com/
```

---

## Maintenance Windows

### Schedule
**Preferred:** Sunday 2-4 AM ET
**Frequency:** Monthly or as needed
**Notice:** 7 days advance

### Checklist
- [ ] Announce maintenance
- [ ] Update status page
- [ ] Create database backup
- [ ] Deploy changes
- [ ] Run smoke tests
- [ ] Verify everything works
- [ ] Close maintenance window
- [ ] Update status page

---

## Tools Summary

**Uptime:** UptimeRobot (free)  
**Errors:** Vercel logs (included)  
**Performance:** Vercel Analytics (included)  
**Database:** Supabase metrics (included)  
**Alerts:** Slack webhooks (free)  
**Status Page:** Statuspage.io or custom  
**Load Testing:** k6 (open source)

**Total Cost:** $0-50/month depending on needs

---

**Questions?** devops@craudiovizai.com
