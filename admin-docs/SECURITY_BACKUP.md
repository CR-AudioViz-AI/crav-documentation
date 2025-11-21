# Security & Backup Procedures

**Admin Reference Guide**  
**Updated:** November 21, 2025 - 4:58 PM EST

---

## Security Procedures

### Access Control
**User Roles:**
- Admin: Full access
- Support: User management, credits
- Dev: Code, deployments
- Billing: Payments only

**Authentication:**
- 2FA required for all admin accounts
- Password requirements: 12+ chars, special chars
- Rotate every 90 days
- No shared accounts

### API Security
**Rate Limiting:**
- Public endpoints: 100 req/min
- Authenticated: 1000 req/min
- Admin: 5000 req/min

**CORS Configuration:**
- Allowed origins: craudiovizai.com, *.vercel.app
- Credentials: true
- Methods: GET, POST, PUT, DELETE

### Database Security
**Row Level Security (RLS):**
- Enabled on all tables
- Users see only their data
- Admin bypass with service key

**Connection Security:**
- SSL required
- Connection pooling (max 100)
- Query timeout: 30s

### Credential Management
**Storage:**
- Vercel environment variables (production)
- .env.local (development only)
- Supabase Vault (sensitive keys)

**Rotation Schedule:**
- API keys: 90 days
- Database passwords: 90 days
- Encryption keys: 365 days
- After any breach: Immediately

---

## Backup Procedures

### Automatic Backups

**Supabase Database:**
- Frequency: Every 6 hours
- Retention: 30 days
- Location: Supabase managed
- Recovery time: <1 hour

**User Files:**
- Frequency: Daily at 2 AM ET
- Retention: 90 days
- Location: AWS S3
- Recovery time: <4 hours

**Code:**
- Location: GitHub (version control)
- All commits backed up
- Recovery: Instant (git checkout)

### Manual Backup

**Before Major Changes:**
```bash
# Database snapshot
npm run backup:database

# User files snapshot
npm run backup:files

# Verify backup
npm run verify:backup
```

**Monthly Full Backup:**
- Complete database export
- All user files
- System configurations
- Store offline (encrypted USB)

---

## Disaster Recovery

### Recovery Time Objectives (RTO)

- Database: <1 hour
- User files: <4 hours
- Full system: <8 hours

### Recovery Point Objectives (RPO)

- Database: 6 hours (last backup)
- User files: 24 hours
- Code: 0 (Git)

### Recovery Procedures

**Database Restoration:**
1. Access Supabase dashboard
2. Navigate to Backups
3. Select restore point
4. Confirm restoration
5. Verify data integrity
6. Notify team

**File Restoration:**
1. Access S3 backup bucket
2. Identify backup date
3. Download files
4. Restore to Supabase Storage
5. Verify file integrity
6. Test file access

**Complete System Recovery:**
1. Deploy code from GitHub
2. Restore database from backup
3. Restore files from S3
4. Configure environment variables
5. Run health checks
6. Gradual traffic restoration

---

## Monitoring

### Health Checks

**Automated:**
- Every 5 minutes
- Check all services
- Alert on failures
- Auto-heal when possible

**Manual (Weekly):**
- Review error logs
- Check performance metrics
- Verify backups
- Security scan

### Alerts

**Critical (Immediate):**
- Database down
- Payment processing failure
- Security breach
- Data corruption

**High (15 min):**
- API errors >5%
- Slow response times
- High error rate
- Storage almost full

**Medium (1 hour):**
- Backup failure
- Performance degradation
- High resource usage

**Low (24 hours):**
- Dependency updates
- General notices

---

## Incident Response

### Steps

1. **Detect:** Alert received or reported
2. **Assess:** Determine severity and impact
3. **Contain:** Stop spread, isolate issue
4. **Notify:** Alert stakeholders
5. **Resolve:** Fix root cause
6. **Verify:** Confirm resolution
7. **Document:** Post-mortem report
8. **Prevent:** Implement safeguards

### Contacts

**Critical Escalation:**
- Roy Henderson (CEO): [phone]
- Dev Team Lead: [phone]
- Security Lead: [phone]

**On-Call Rotation:**
- View schedule: admin.craudiovizai.com/on-call

---

## Compliance

### GDPR

- Data encryption at rest
- Right to be forgotten (30 days)
- Data export capability
- Privacy policy updated

### SOC 2

- Access logs maintained
- Security policies documented
- Audit trail complete
- Annual assessment

---

## Security Checklist

**Daily:**
- [ ] Review access logs
- [ ] Check failed login attempts
- [ ] Monitor API usage

**Weekly:**
- [ ] Backup verification
- [ ] Security scan
- [ ] Update dependencies

**Monthly:**
- [ ] Full security audit
- [ ] Access review
- [ ] Policy updates

**Quarterly:**
- [ ] Penetration testing
- [ ] Disaster recovery drill
- [ ] Credential rotation

---

**Questions?** security@craudiovizai.com

*Fortune 50 Security Standards Applied*
