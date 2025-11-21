# BOT SYSTEM - COMPLETE DOCUMENTATION
**CR AudioViz AI Autonomous Monitoring System**

**Last Updated:** November 21, 2025 - 4:10 PM EST  
**Status:** 100% Operational - 9 Bots Running 24/7  
**Repository:** CR-AudioViz-AI/craudiovizai-website  
**Admin Dashboard:** https://craudiovizai.com/admin/bots

---

## EXECUTIVE SUMMARY

**What It Is:**
24/7 autonomous monitoring system with 9 specialized bots that continuously monitor, detect issues, self-heal, and escalate problems to Javari AI when needed.

**Current Status:**
- 9 bots: OPERATIONAL
- Cron jobs: CONFIGURED
- Database: COMPLETE
- Self-healing: ACTIVE
- Javari integration: READY
- Admin dashboard: LIVE

**Business Value:**
- Reduces downtime by detecting issues before users notice
- Saves support costs through automatic resolution
- Provides 24/7 coverage without human staff
- Builds knowledge base for continuous improvement
- Escalates complex issues to Javari AI automatically

---

## THE 9 BOTS

### 1. CONDUCTOR (Master Orchestrator)
**Role:** Coordinates all other bots, prioritizes tasks, manages escalations

**Schedule:** Every 5 minutes  
**Cron:** `*/5 * * * *`  
**API Route:** `/api/bots/conductor`

**Responsibilities:**
- Monitor status of all 8 other bots
- Prioritize detected issues by severity
- Coordinate bot responses to complex problems
- Escalate high-priority issues to Javari AI
- Track overall system health score
- Generate daily status reports

**Key Metrics:**
- Bots monitored: 8
- Issues coordinated: Tracked
- Escalations made: Tracked
- System health score: 0-100

**Self-Healing Capabilities:**
- Restart failed bots
- Reprioritize tasks during high load
- Balance monitoring load across bots

---

### 2. SENTINEL (Site Health Monitor)
**Role:** Monitor main website health and availability

**Schedule:** Every 2 minutes  
**Cron:** `*/2 * * * *`  
**API Route:** `/api/bots/sentinel`

**Checks:**
- Homepage response time
- Critical page availability (pricing, docs, apps)
- HTTP status codes
- Response time SLA (target: <2 seconds)
- Error rates (target: <1%)

**Alerts On:**
- Site down (503, 500 errors)
- Slow response times (>5 seconds)
- High error rates (>5%)
- SSL certificate expiration (<30 days)

**Auto-Fixes:**
- Clear cache on 500 errors
- Restart services if possible
- Create ticket for human review

---

### 3. SENTINEL-API (API Monitor)
**Role:** Monitor all API endpoints for health and performance

**Schedule:** Every 3 minutes  
**Cron:** `*/3 * * * *`  
**API Route:** `/api/bots/sentinel-api`

**Monitors:**
- All `/api/*` endpoints
- Response times per endpoint
- Success rates per endpoint
- Rate limit compliance
- CORS configuration

**Tests:**
- Authentication flows
- Data retrieval endpoints
- Write operations (safe test data)
- Error handling
- Response format validation

**Auto-Fixes:**
- Retry failed requests
- Clear API caches
- Reset rate limit counters (if safe)

---

### 4. GUARDIAN (Payment Monitor)
**Role:** Monitor payment processing systems

**Schedule:** Every 5 minutes  
**Cron:** `*/5 * * * *`  
**API Route:** `/api/bots/guardian`

**Monitors:**
- Stripe API health
- PayPal API health
- Webhook delivery
- Failed payments
- Subscription status
- Credit system accuracy

**Checks:**
- Payment processing success rate
- Average transaction time
- Webhook delivery confirmation
- Refund processing
- Subscription renewals

**Alerts On:**
- Payment processor down
- Webhook failures (>5%)
- Failed payment spike (>10%)
- Credit calculation errors

**Auto-Fixes:**
- Retry failed webhooks
- Reconcile credit balances
- Generate payment error reports

---

### 5. SENTINEL-DB (Database Monitor)
**Role:** Monitor Supabase database health

**Schedule:** Every 10 minutes  
**Cron:** `*/10 * * * *`  
**API Route:** `/api/bots/sentinel-db`

**Monitors:**
- Connection pool health
- Query performance
- Storage usage
- Connection count
- Slow query detection (>1 second)

**Checks:**
- Can connect to database
- Can read from all tables
- Can write to test table
- Backup status
- Replication lag (if applicable)

**Alerts On:**
- Connection failures
- Storage >80% full
- Slow queries >100/hour
- Backup failures
- Replication lag >5 minutes

**Auto-Fixes:**
- Clear connection pool
- Vacuum database if needed
- Archive old records

---

### 6. ARCHITECT (Build Monitor)
**Role:** Monitor GitHub and Vercel build/deployment health

**Schedule:** Every 15 minutes  
**Cron:** `*/15 * * * *`  
**API Route:** `/api/bots/architect`

**Monitors:**
- GitHub repository health
- Recent commits
- Vercel deployment status
- Build success rates
- Build durations

**Checks:**
- Latest deployments per project
- Build failure patterns
- TypeScript errors
- Dependency vulnerabilities
- Code quality metrics

**Alerts On:**
- Build failures (>3 in row)
- Deployment stuck >30 minutes
- Critical vulnerabilities
- Repository issues

**Auto-Fixes:**
- Retry failed deployments
- Clear build cache
- Generate build error reports

---

### 7. ORACLE (Security Monitor)
**Role:** Monitor security and detect threats

**Schedule:** Every 30 minutes  
**Cron:** `*/30 * * * *`  
**API Route:** `/api/bots/oracle`

**Monitors:**
- Unusual login patterns
- Failed authentication attempts
- API abuse (rate limiting violations)
- Suspicious user behavior
- Security vulnerabilities

**Checks:**
- Failed login attempts per IP
- API requests per user
- Database access patterns
- Admin action logs
- Dependency vulnerabilities

**Alerts On:**
- Brute force attempts (>5 failures)
- API abuse (>1000 requests/min)
- Unauthorized admin access attempts
- Critical CVEs in dependencies

**Auto-Fixes:**
- Block suspicious IPs (temporary)
- Rate limit abusive users
- Force password reset if needed
- Generate security reports

---

### 8. SCOUT (Competitive Intelligence)
**Role:** Monitor competitors and industry trends

**Schedule:** Every 6 hours  
**Cron:** `0 */6 * * *`  
**API Route:** `/api/bots/scout`

**Monitors:**
- Competitor product updates
- Competitor pricing changes
- Industry news and trends
- Technology trends
- Market analysis

**Data Sources:**
- NewsAPI (100 calls/day)
- Competitor websites
- Social media mentions
- Product Hunt
- Tech news sites

**Generates:**
- Daily competitor summary
- Weekly trend analysis
- Monthly market report
- Strategic recommendations

**Feeds Data To:**
- Javari AI for market intelligence
- Admin dashboard for review
- Strategic planning reports

---

### 9. PULSE (User Activity Monitor)
**Role:** Monitor real-time user activity and system load

**Schedule:** Every 1 minute  
**Cron:** `*/1 * * * *`  
**API Route:** `/api/bots/pulse`

**Monitors:**
- Active users (current)
- Request rate (per minute)
- Server load (CPU, memory)
- Response times (average)
- Error rates (real-time)

**Metrics:**
- Concurrent users
- Requests per minute
- Average response time
- Error percentage
- System resources

**Alerts On:**
- High load (>80% CPU)
- Memory issues (>90% used)
- Error spike (>5% errors)
- Slow responses (>3s average)

**Auto-Fixes:**
- Clear caches on high load
- Gracefully reject new requests if overloaded
- Scale resources if possible

---

## DATABASE SCHEMA

### Table: `bots`
**Purpose:** Store bot definitions and configuration

```sql
CREATE TABLE bots (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(50) UNIQUE NOT NULL,
  display_name VARCHAR(100) NOT NULL,
  description TEXT,
  schedule_cron VARCHAR(50) NOT NULL,
  api_endpoint VARCHAR(200) NOT NULL,
  status VARCHAR(20) DEFAULT 'active', -- active, paused, error
  last_execution_at TIMESTAMPTZ,
  next_execution_at TIMESTAMPTZ,
  total_runs INTEGER DEFAULT 0,
  successful_runs INTEGER DEFAULT 0,
  failed_runs INTEGER DEFAULT 0,
  avg_execution_time_ms INTEGER,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Table: `bot_executions`
**Purpose:** Log every bot execution

```sql
CREATE TABLE bot_executions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  bot_id UUID REFERENCES bots(id) ON DELETE CASCADE,
  status VARCHAR(20) NOT NULL, -- running, success, failed
  started_at TIMESTAMPTZ NOT NULL,
  completed_at TIMESTAMPTZ,
  execution_time_ms INTEGER,
  checks_performed INTEGER DEFAULT 0,
  issues_found INTEGER DEFAULT 0,
  actions_taken INTEGER DEFAULT 0,
  error_message TEXT,
  metadata JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Table: `bot_findings`
**Purpose:** Track issues detected by bots

```sql
CREATE TABLE bot_findings (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  bot_id UUID REFERENCES bots(id) ON DELETE CASCADE,
  execution_id UUID REFERENCES bot_executions(id) ON DELETE CASCADE,
  severity VARCHAR(20) NOT NULL, -- info, warning, error, critical
  category VARCHAR(50) NOT NULL,
  title VARCHAR(200) NOT NULL,
  description TEXT,
  affected_component VARCHAR(100),
  detected_at TIMESTAMPTZ DEFAULT NOW(),
  resolved_at TIMESTAMPTZ,
  resolution_method VARCHAR(100), -- auto-fixed, manual, escalated
  metadata JSONB
);
```

### Table: `bot_actions`
**Purpose:** Track actions taken by bots

```sql
CREATE TABLE bot_actions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  bot_id UUID REFERENCES bots(id) ON DELETE CASCADE,
  finding_id UUID REFERENCES bot_findings(id) ON DELETE SET NULL,
  action_type VARCHAR(50) NOT NULL,
  action_description TEXT NOT NULL,
  success BOOLEAN NOT NULL,
  error_message TEXT,
  taken_at TIMESTAMPTZ DEFAULT NOW(),
  metadata JSONB
);
```

### Table: `bot_tickets`
**Purpose:** Escalations to Javari AI or human support

```sql
CREATE TABLE bot_tickets (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  bot_id UUID REFERENCES bots(id) ON DELETE CASCADE,
  finding_id UUID REFERENCES bot_findings(id) ON DELETE CASCADE,
  severity VARCHAR(20) NOT NULL,
  title VARCHAR(200) NOT NULL,
  description TEXT NOT NULL,
  status VARCHAR(20) DEFAULT 'open', -- open, assigned, resolved, closed
  assigned_to VARCHAR(50), -- 'javari' or user_id
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  resolved_at TIMESTAMPTZ,
  resolution_notes TEXT,
  metadata JSONB
);
```

### Table: `bot_knowledge`
**Purpose:** Self-healing knowledge base

```sql
CREATE TABLE bot_knowledge (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  category VARCHAR(50) NOT NULL,
  issue_pattern TEXT NOT NULL,
  solution TEXT NOT NULL,
  success_rate DECIMAL(5,2) DEFAULT 0.00,
  times_applied INTEGER DEFAULT 0,
  times_successful INTEGER DEFAULT 0,
  last_used_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Table: `bot_metrics`
**Purpose:** Aggregate metrics for dashboards

```sql
CREATE TABLE bot_metrics (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  metric_date DATE NOT NULL,
  bot_id UUID REFERENCES bots(id) ON DELETE CASCADE,
  total_executions INTEGER DEFAULT 0,
  successful_executions INTEGER DEFAULT 0,
  failed_executions INTEGER DEFAULT 0,
  avg_execution_time_ms INTEGER,
  issues_detected INTEGER DEFAULT 0,
  issues_resolved INTEGER DEFAULT 0,
  tickets_created INTEGER DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(metric_date, bot_id)
);
```

---

## VERCEL CRON CONFIGURATION

**File:** `vercel.json`

```json
{
  "crons": [
    {
      "path": "/api/bots/pulse",
      "schedule": "*/1 * * * *"
    },
    {
      "path": "/api/bots/sentinel",
      "schedule": "*/2 * * * *"
    },
    {
      "path": "/api/bots/sentinel-api",
      "schedule": "*/3 * * * *"
    },
    {
      "path": "/api/bots/conductor",
      "schedule": "*/5 * * * *"
    },
    {
      "path": "/api/bots/guardian",
      "schedule": "*/5 * * * *"
    },
    {
      "path": "/api/bots/sentinel-db",
      "schedule": "*/10 * * * *"
    },
    {
      "path": "/api/bots/architect",
      "schedule": "*/15 * * * *"
    },
    {
      "path": "/api/bots/oracle",
      "schedule": "*/30 * * * *"
    },
    {
      "path": "/api/bots/scout",
      "schedule": "0 */6 * * *"
    }
  ]
}
```

---

## JAVARI AI INTEGRATION

### How Bots Escalate to Javari

**Workflow:**
1. Bot detects issue beyond auto-fix capability
2. Bot creates ticket in `bot_tickets` table
3. Bot marks ticket as assigned to 'javari'
4. Javari AI monitoring picks up ticket
5. Javari analyzes issue with full context
6. Javari attempts resolution or escalates to human
7. Resolution logged back to ticket

**Data Shared with Javari:**
- Complete issue description
- Recent execution logs
- Related findings
- Previous attempted fixes
- System state at time of issue

---

## ADMIN DASHBOARD

**URL:** https://craudiovizai.com/admin/bots

**Features:**
- Real-time bot status (all 9 bots)
- Execution history (last 100 runs)
- Issues detected (grouped by severity)
- Actions taken (success/failure)
- Open tickets (needs attention)
- Performance metrics (response times)
- Manual bot triggers (test runs)

**Views:**
- Overview: All bots at a glance
- Per-Bot Details: Deep dive into one bot
- Issues: All detected issues
- Tickets: All escalations
- Metrics: Performance over time

---

## SELF-HEALING SYSTEM

### How It Works

**Knowledge Base:**
- Every issue and resolution is stored
- Success rates tracked per solution
- Most effective solutions prioritized
- Patterns recognized over time

**Learning Process:**
1. Bot encounters issue
2. Checks knowledge base for similar patterns
3. Applies known solution if found
4. Tracks success/failure
5. Updates knowledge base with result
6. If no known solution, creates ticket

**Example Knowledge:**
```
Issue Pattern: "503 Service Unavailable"
Solution: "Clear Next.js cache and restart"
Success Rate: 87%
Times Applied: 23
Times Successful: 20
```

---

## DEPLOYMENT GUIDE

### Initial Setup (One-Time)

**1. Database Migration**
```bash
# Run this SQL in Supabase
# See: /admin-docs/bot-system-migration.sql
```

**2. Environment Variables** (Already configured)
```
SUPABASE_SERVICE_ROLE_KEY=[set]
NEXT_PUBLIC_SUPABASE_URL=[set]
```

**3. Deploy to Vercel**
```bash
# Already deployed - cron jobs active
```

---

## MONITORING THE BOTS

### How to Check Bot Health

**Option 1: Admin Dashboard**
1. Go to https://craudiovizai.com/admin/bots
2. View real-time status
3. Check recent executions
4. Review any issues

**Option 2: Database Queries**
```sql
-- Check all bot status
SELECT name, status, last_execution_at, total_runs, 
       successful_runs, failed_runs
FROM bots
ORDER BY name;

-- Check recent executions
SELECT b.name, e.status, e.started_at, 
       e.execution_time_ms, e.issues_found
FROM bot_executions e
JOIN bots b ON b.id = e.bot_id
ORDER BY e.started_at DESC
LIMIT 20;

-- Check open tickets
SELECT b.name, t.severity, t.title, t.created_at
FROM bot_tickets t
JOIN bots b ON b.id = t.bot_id
WHERE t.status = 'open'
ORDER BY t.created_at DESC;
```

**Option 3: Vercel Logs**
1. Go to Vercel dashboard
2. Select craudiovizai-website project
3. View Functions logs
4. Filter by bot API routes

---

## TROUBLESHOOTING

### Bot Not Running

**Symptoms:** Last execution timestamp not updating

**Causes:**
1. Vercel cron job paused
2. API route error
3. Database connection issue

**Solutions:**
1. Check Vercel cron job status
2. View bot execution logs in Vercel
3. Test bot API route manually
4. Check database connectivity

### Bot Failing

**Symptoms:** High failed_runs count

**Causes:**
1. External service down (API, database)
2. Bug in bot logic
3. Environment variable missing

**Solutions:**
1. Check `bot_executions` table for error messages
2. Review Vercel function logs
3. Test bot manually via admin dashboard
4. Verify all environment variables set

### Too Many Tickets

**Symptoms:** Lots of open tickets, Javari overwhelmed

**Causes:**
1. Real systemic issue
2. Bot false positives
3. Knowledge base gaps

**Solutions:**
1. Review ticket patterns
2. Adjust bot detection thresholds
3. Add solutions to knowledge base
4. Fix underlying systemic issues

---

## BEST PRACTICES

### For Roy (Admin)

**Daily:**
- Check admin dashboard (2 minutes)
- Review any critical tickets
- Verify bots ran in last hour

**Weekly:**
- Review bot metrics
- Check success rates
- Add to knowledge base if needed

**Monthly:**
- Review bot effectiveness
- Adjust schedules if needed
- Archive resolved tickets

### For Bot Development

**Adding New Bots:**
1. Create bot entry in database
2. Build API route following existing pattern
3. Add cron job to vercel.json
4. Deploy and test
5. Monitor first 24 hours closely

**Modifying Existing Bots:**
1. Test changes locally first
2. Deploy to preview
3. Monitor metrics before production
4. Update documentation

---

## METRICS & KPIs

**System Health:**
- Uptime: 99.9% target
- Issues detected per day: Tracked
- Issues auto-resolved: >80% target
- Mean time to detection (MTTD): <5 minutes
- Mean time to resolution (MTTR): <15 minutes

**Bot Performance:**
- Execution success rate: >95% target
- Average execution time: <10 seconds
- False positive rate: <5% target
- Knowledge base hit rate: Tracked

**Business Impact:**
- Downtime prevented: Tracked
- Support tickets avoided: Tracked
- Cost savings: Calculated
- User impact: Minimized

---

## FUTURE ENHANCEMENTS

**Planned:**
- Machine learning for pattern detection
- Predictive issue prevention
- Automated scaling decisions
- Integration with more services
- Mobile app for alerts
- Slack/Discord notifications

---

## SUMMARY FOR NEW TEAM MEMBERS

**What You Need to Know:**
1. 9 bots monitor everything 24/7
2. Most issues auto-fix without human intervention
3. Complex issues escalate to Javari AI
4. Check admin dashboard daily
5. Bots learn and improve over time
6. System is 100% operational

**Your Role:**
- Review tickets that Javari can't fix
- Add knowledge to help bots learn
- Adjust bot sensitivity as needed
- Monitor overall system health

---

**END OF BOT SYSTEM DOCUMENTATION**

**Questions?** Check admin dashboard or ask Javari AI.

**Last Updated:** November 21, 2025 - 4:10 PM EST
