# CR AudioViz AI - Troubleshooting Guide

**Version:** 3.0  
**Last Updated:** November 21, 2025 - 12:00 PM EST  
**Audience:** Support Team, Admins, Power Users

---

## Quick Diagnosis

### Issue Categories
1. **Authentication** - Can't log in/sign up
2. **Performance** - Slow or timing out
3. **Features** - Tool not working correctly
4. **Payments** - Billing or credit issues
5. **Data** - Missing or incorrect data
6. **Integrations** - External services failing

### Severity Levels
- **P0 (Critical)**: Platform down, affects all users
- **P1 (High)**: Major feature broken, affects many users
- **P2 (Medium)**: Feature degraded, workaround exists
- **P3 (Low)**: Minor issue, cosmetic, or rare edge case

---

## Authentication Issues

### Can't Sign Up

**Symptom:** "Email already registered" or sign up fails

**Check:**
1. Is email already in system?
```sql
SELECT email, created_at FROM auth.users 
WHERE email = 'user@example.com';
```

2. Check for partial registration
```sql
SELECT * FROM user_profiles 
WHERE email = 'user@example.com';
```

**Solutions:**
- Email exists → User should use "Forgot Password"
- Partial registration → Complete via password reset
- Supabase down → Check status.supabase.com

**Prevention:**
- Better error messages
- Email verification before full registration
- Cleanup stale partial registrations

### Can't Log In

**Symptom:** "Invalid credentials" or infinite loading

**Check:**
1. Verify user exists:
```sql
SELECT id, email, email_confirmed_at, last_sign_in_at
FROM auth.users
WHERE email = 'user@example.com';
```

2. Check for locked account:
```sql
SELECT * FROM auth_logs
WHERE user_id = 'xxx' AND success = false
ORDER BY created_at DESC LIMIT 10;
```

**Solutions:**
- Wrong password → Password reset
- Email not confirmed → Resend confirmation
- Account locked → Manual unlock in admin panel
- Session expired → Clear cookies and retry

**Manual Unlock:**
```sql
UPDATE auth.users
SET banned_until = NULL
WHERE id = 'user_id';
```

### Session Expires Too Fast

**Symptom:** Logged out after 5-10 minutes

**Check Supabase config:**
```javascript
// Should be 1 week minimum
const { data, error } = await supabase.auth.getSession();
console.log('Session expires:', data.session.expires_at);
```

**Fix:**
```javascript
// Update auth config
await supabase.auth.updateUser({
  options: {
    autoRefreshToken: true,
    persistSession: true
  }
});
```

---

## Performance Issues

### Slow Page Load

**Symptom:** Takes > 5 seconds to load

**Check:**
1. **Network:**
```bash
# Test from multiple locations
curl -o /dev/null -s -w '%{time_total}\n' https://craudiovizai.com
```

2. **Database:**
```sql
SELECT query, total_time, calls
FROM pg_stat_statements
ORDER BY total_time DESC LIMIT 10;
```

3. **API:**
```sql
SELECT endpoint, AVG(response_time) as avg_ms
FROM api_metrics
WHERE timestamp > NOW() - INTERVAL '1 hour'
GROUP BY endpoint
ORDER BY avg_ms DESC;
```

**Common Causes:**
- Unoptimized queries (missing indexes)
- Large images not compressed
- No caching headers
- Too many API calls on page load
- Serverless cold starts

**Quick Fixes:**
- Add database indexes
- Implement CDN caching
- Lazy load images
- Reduce bundle size
- Warm up functions

**Database Optimization:**
```sql
-- Add index for common queries
CREATE INDEX CONCURRENTLY idx_projects_user_id 
ON projects(user_id);

-- Analyze query plan
EXPLAIN ANALYZE
SELECT * FROM projects WHERE user_id = 'xxx';
```

### API Timeouts

**Symptom:** "Request timeout" or 504 errors

**Check Vercel logs:**
```bash
vercel logs --filter=/api/javari/chat
```

**Common Causes:**
- Function taking > 60 seconds (Vercel limit)
- AI provider slow response
- Database query hanging
- Network issues

**Solutions:**
- Increase function timeout (vercel.json)
- Add timeout handling in code
- Implement retry logic
- Stream responses for long operations

**Code Fix:**
```typescript
// Add timeout
const controller = new AbortController();
const timeoutId = setTimeout(() => controller.abort(), 30000);

try {
  const response = await fetch(url, {
    signal: controller.signal
  });
} finally {
  clearTimeout(timeoutId);
}
```

---

## Feature Issues

### Tool Not Loading

**Symptom:** Blank screen or "Something went wrong"

**Check browser console:**
- Open DevTools (F12)
- Look for red errors
- Check Network tab for failed requests

**Common Errors:**
```
TypeError: Cannot read property 'x' of undefined
→ Missing data from API

Failed to load resource: 401 Unauthorized
→ Session expired or invalid token

Module not found
→ Build/deploy issue
```

**Quick Fixes:**
- Refresh page (clears state)
- Clear browser cache
- Log out and back in
- Try incognito mode
- Check if Vercel deployment succeeded

**If Tool Consistently Fails:**
1. Check tool status:
```sql
SELECT tool_name, enabled, error_count
FROM tool_status
WHERE updated_at > NOW() - INTERVAL '1 hour';
```

2. Check recent deployments:
```bash
vercel ls <project-name>
```

3. Review error logs:
```sql
SELECT * FROM error_logs
WHERE tool_name = 'logo-maker'
ORDER BY created_at DESC LIMIT 20;
```

### AI Not Responding

**Symptom:** Message sent but no response

**Check:**
1. **API Key Valid:**
```bash
# Test OpenAI
curl https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{"model":"gpt-4","messages":[{"role":"user","content":"test"}]}'

# Test Anthropic
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -d '{"model":"claude-sonnet-4","max_tokens":100,"messages":[...]}'
```

2. **Rate Limits:**
```sql
SELECT provider, COUNT(*) as requests_today
FROM ai_request_logs
WHERE created_at > NOW() - INTERVAL '24 hours'
GROUP BY provider;
```

3. **Cost Limits:**
```sql
SELECT SUM(total_cost) as total_spend_today
FROM ai_request_logs
WHERE created_at > NOW() - INTERVAL '24 hours';
```

**Solutions:**
- API key expired → Rotate key
- Rate limited → Use backup provider
- Cost limit hit → Increase budget or wait
- Provider down → Check status pages

### Credits Not Deducting

**Symptom:** Tool used but credits unchanged

**Check transaction logs:**
```sql
SELECT * FROM credit_transactions
WHERE user_id = 'xxx'
ORDER BY created_at DESC LIMIT 20;
```

**Possible Causes:**
- Transaction failed to record
- Webhook not firing
- Database write failed
- Race condition

**Manual Fix:**
```sql
-- Deduct credits manually
UPDATE user_profiles
SET credits = credits - 25
WHERE user_id = 'xxx';

-- Record transaction
INSERT INTO credit_transactions (user_id, amount, type, description)
VALUES ('xxx', -25, 'tool_use', 'Manual adjustment - logo maker');
```

---

## Payment Issues

### Payment Fails

**Symptom:** "Payment declined" or "Something went wrong"

**Check Stripe Dashboard:**
1. Find payment intent
2. Check failure reason
3. Review customer payment methods

**Common Reasons:**
- Insufficient funds
- Card expired
- Incorrect CVC
- Bank declined (fraud prevention)
- International card blocked

**Solutions:**
- Ask user to try different card
- Verify billing address
- Contact bank if fraud block
- Use PayPal as alternative

### Subscription Not Activating

**Symptom:** Payment succeeded but still on Free plan

**Check:**
```sql
SELECT subscription_status, stripe_subscription_id, plan
FROM user_profiles
WHERE user_id = 'xxx';
```

**Verify in Stripe:**
```bash
curl https://api.stripe.com/v1/subscriptions/sub_xxx \
  -u sk_live_...:
```

**Possible Causes:**
- Webhook not received
- Subscription ID not saved
- Database update failed

**Manual Fix:**
```sql
UPDATE user_profiles
SET 
  plan = 'pro',
  subscription_status = 'active',
  stripe_subscription_id = 'sub_xxx'
WHERE user_id = 'xxx';
```

### Credits Not Added After Purchase

**Symptom:** Payment succeeded but credits unchanged

**Check payment logs:**
```sql
SELECT * FROM payment_logs
WHERE user_id = 'xxx'
ORDER BY created_at DESC LIMIT 10;
```

**Check webhook delivery:**
- Stripe Dashboard → Webhooks → View logs
- Look for `checkout.session.completed` event

**Manual Credit Addition:**
```sql
BEGIN;

UPDATE user_profiles
SET credits = credits + 1000
WHERE user_id = 'xxx';

INSERT INTO credit_transactions (user_id, amount, type, description, payment_id)
VALUES ('xxx', 1000, 'purchase', 'Manual addition - Stripe webhook failed', 'pi_xxx');

COMMIT;
```

---

## Data Issues

### Missing Projects

**Symptom:** User says projects disappeared

**Check soft deletes:**
```sql
SELECT id, name, deleted_at
FROM projects
WHERE user_id = 'xxx' AND deleted_at IS NOT NULL
ORDER BY deleted_at DESC;
```

**Check archive:**
```sql
SELECT * FROM projects
WHERE user_id = 'xxx' AND archived = true;
```

**Restore if needed:**
```sql
UPDATE projects
SET deleted_at = NULL, archived = false
WHERE id = 'proj_xxx';
```

### Data Not Syncing

**Symptom:** Changes in one place don't appear elsewhere

**Check:**
1. Browser console for errors
2. Network tab for failed requests
3. Database for actual current value

**Common Causes:**
- Client-side state stale
- Optimistic update failed
- WebSocket disconnected
- Database write failed

**Solutions:**
- Force refresh data
- Clear local storage
- Reconnect WebSocket
- Check database directly

---

## Integration Issues

### GitHub Integration Broken

**Symptom:** Can't push to GitHub or deploy

**Check token:**
```bash
curl -H "Authorization: token ghp_xxx" \
  https://api.github.com/user
```

**Solutions:**
- Token expired → Generate new token
- Wrong permissions → Add repo, workflow permissions
- Rate limited → Wait or use different token

### Vercel Deployment Failing

**Symptom:** Build fails or doesn't deploy

**Check:**
1. GitHub Actions status
2. Vercel deployment logs
3. Environment variables set

**Common Failures:**
```bash
# TypeScript errors
npm run type-check

# Build errors
npm run build

# Environment variables missing
vercel env ls
```

**Quick Fixes:**
- Fix TypeScript errors first
- Verify all env vars in Vercel dashboard
- Check vercel.json configuration
- Try manual deploy: `vercel --prod`

---

## Emergency Procedures

### Platform Down (P0)

**Immediate Actions:**
1. Check status page
2. Alert team
3. Roll back last deployment
4. Monitor error rates

**Rollback:**
```bash
vercel rollback <previous-deployment-url> --prod
```

**Communication:**
```
Subject: Platform Issue - We're On It

We're aware of an issue affecting [specific feature/entire platform]. 
Our team is actively working on a fix.

Status updates: https://status.craudiovizai.com
ETA: [X minutes]

We apologize for the inconvenience.
```

### Database Issues

**If Supabase unresponsive:**
1. Check status.supabase.com
2. Verify connection strings
3. Check connection pool limits
4. Contact Supabase support

**If queries hanging:**
```sql
-- Find long-running queries
SELECT pid, now() - query_start as duration, query
FROM pg_stat_activity
WHERE state = 'active'
ORDER BY duration DESC;

-- Kill if needed (use carefully!)
SELECT pg_terminate_backend(pid);
```

---

## Prevention Checklist

**Before Every Deploy:**
- [ ] All tests passing
- [ ] TypeScript compiles with no errors
- [ ] Environment variables verified
- [ ] Database migrations tested
- [ ] Rollback plan documented

**Weekly Monitoring:**
- [ ] Review error logs
- [ ] Check performance metrics
- [ ] Verify bot health
- [ ] Review support tickets
- [ ] Update documentation

**Monthly Review:**
- [ ] Analyze incident trends
- [ ] Update troubleshooting guides
- [ ] Train team on new issues
- [ ] Improve monitoring/alerts
- [ ] Optimize common problems

---

## Support Escalation

**Tier 1 (Self-Service):**
- Knowledge base articles
- Video tutorials
- Community forums

**Tier 2 (Support Team):**
- Live chat support
- Email support
- Standard troubleshooting

**Tier 3 (Engineering):**
- Complex technical issues
- Data recovery
- Custom solutions
- Roy Henderson (CEO)

**Contact:**
- General Support: support@craudiovizai.com
- Technical Issues: tech@craudiovizai.com
- Urgent (P0/P1): roy@craudiovizai.com

---

**END OF TROUBLESHOOTING GUIDE**
