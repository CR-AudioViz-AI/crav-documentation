# Escalation Workflows

**Helpdesk Reference**  
**Updated:** November 21, 2025 - 5:01 PM EST

---

## When to Escalate

### Tier 1 → Tier 2 (Senior Support)
- Can't resolve in 30 minutes
- Customer requests supervisor
- Refund outside policy requires approval
- Complex technical issue
- Account restoration needed

### Tier 2 → Engineering
- Confirmed bug affecting multiple users
- Database/performance issues
- API failures
- Build/deployment problems
- Security concerns

### Any Tier → Management
- Legal threat
- Media inquiry
- Major service outage
- Data breach suspected
- Fraud detected

---

## Escalation Process

### Step 1: Document
**Required Info:**
- Ticket ID
- Customer email
- Issue description
- Steps already taken
- Impact (users affected)
- Urgency level

### Step 2: Notify
**Methods:**
- Slack: #escalations channel
- Email: escalations@craudiovizai.com
- Phone: Critical issues only

**Template:**
```
ESCALATION: [TICKET-ID]
Customer: [email]
Issue: [brief description]
Attempted: [what you tried]
Impact: [number of users]
Urgency: [Low/Medium/High/Critical]
```

### Step 3: Handoff
- Tag in ticket system
- Brief new assignee
- Stay available for questions
- Monitor progress

### Step 4: Follow Up
- Check status every 4 hours
- Update customer every 24 hours
- Close loop when resolved

---

## Urgency Levels

### Critical (Immediate)
**Response:** <15 minutes  
**Examples:**
- Payment processing down
- Complete service outage
- Data breach
- Security vulnerability

**Actions:**
1. Alert on-call engineer immediately
2. Notify management
3. Create incident channel
4. Update status page

### High (30 minutes)
**Response:** <30 minutes  
**Examples:**
- Tool completely broken
- Multiple users affected
- Revenue-impacting issue
- Data loss risk

**Actions:**
1. Escalate to senior support
2. Document thoroughly
3. Provide workaround if available
4. Monitor closely

### Medium (2 hours)
**Response:** <2 hours  
**Examples:**
- Single user complex issue
- Feature not working correctly
- Performance degradation
- Integration problem

**Actions:**
1. Attempt resolution
2. Escalate if stuck
3. Regular customer updates
4. Document solution

### Low (24 hours)
**Response:** <24 hours  
**Examples:**
- Feature requests
- Enhancement ideas
- Minor bugs
- General questions

**Actions:**
1. Log in backlog
2. Respond to customer
3. No immediate escalation
4. Review weekly

---

## Escalation Contacts

### Tier 2 - Senior Support
**Slack:** #senior-support  
**Email:** senior-support@craudiovizai.com  
**Hours:** 24/7 (rotation)

### Engineering Team
**Slack:** #engineering  
**Email:** engineering@craudiovizai.com  
**Hours:** 9 AM - 6 PM ET (on-call after hours)

### Management
**Roy Henderson (CEO):**  
Email: roy@craudiovizai.com  
Phone: [critical only]  
**Use for:** Legal, media, major incidents

**Cindy Henderson (CMO):**  
Email: cindy@craudiovizai.com  
**Use for:** Marketing issues, partnerships

### On-Call Rotation
**Current:** View at admin.craudiovizai.com/on-call  
**Schedule:** Weekly rotation  
**Contact:** Slack DM or phone for Critical issues

---

## Special Escalations

### Security Issues
**Process:**
1. Do NOT reply to customer yet
2. Alert security@craudiovizai.com immediately
3. Follow security incident playbook
4. Management approval required for communication

**Never:**
- Confirm or deny breach
- Share technical details
- Promise specific timelines

### Legal Threats
**Process:**
1. Forward to legal@craudiovizai.com
2. Do NOT engage with customer
3. Note in ticket: "Escalated to Legal"
4. Let legal team handle communication

**Never:**
- Admit fault
- Make promises
- Discuss legal matters

### Media Inquiries
**Process:**
1. Forward to media@craudiovizai.com
2. Note in ticket: "Media Inquiry"
3. Management handles response
4. Never comment directly

### Fraud Suspected
**Process:**
1. Alert fraud@craudiovizai.com
2. Suspend account (if needed)
3. Document evidence
4. Don't alert customer

---

## Escalation Templates

### To Senior Support
```
Hi [Name],

Escalating ticket [ID] for [customer email].

Issue: [brief description]
Attempted: [what I tried]
Blocker: [why I can't resolve]

Customer expecting update by: [time]

Thanks!
```

### To Engineering
```
Subject: BUG - [Brief Description]

Ticket: [ID]
Severity: [Low/Medium/High/Critical]
Users Affected: [number]

Description:
[Detailed issue description]

Steps to Reproduce:
1. [step]
2. [step]
3. [step]

Expected: [what should happen]
Actual: [what happens]

Workaround: [if available]
Logs: [attach or link]
```

### To Management
```
Subject: ESCALATION - [Issue Type]

Ticket: [ID]
Customer: [email/name]
Issue Type: [Legal/Media/Security/Major Outage]

Summary:
[Brief overview]

Current Status:
[What's happening]

Recommended Action:
[Your suggestion]

Urgency: [Why this needs attention]
```

---

## De-Escalation

### When to De-Escalate
- Issue resolved
- Customer satisfied
- Workaround accepted
- Lower priority now

### Process
1. Confirm resolution with customer
2. Document solution
3. Update ticket
4. Thank escalation team
5. Close ticket

---

## Escalation Metrics

### Track
- Escalation rate (target: <10%)
- Time to resolve after escalation
- Customer satisfaction post-escalation
- Root causes (improve training)

### Review
- Weekly: Escalation trends
- Monthly: Root cause analysis
- Quarterly: Process improvements

---

## Training

### For New Support Staff
- Shadow escalations
- Practice scenarios
- Know contacts
- Understand urgency levels

### Ongoing
- Review escalation cases
- Share learnings
- Update procedures
- Improve documentation

---

**Questions?** support-training@craudiovizai.com
