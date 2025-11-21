# Common Issues & Solutions

**Quick Reference Guide for Support Team**  
**Updated:** November 21, 2025 - 4:55 PM EST

---

## Account & Login (40% of tickets)

### Can't Login
**Symptoms:** Wrong password, email not recognized  
**Fix:**
1. Send password reset: Account → Reset Password
2. Check for typos in email
3. Search database: Admin → Users → Search
4. If account doesn't exist: Suggest signup

### Credits Not Showing
**Symptoms:** Purchased but balance unchanged  
**Fix:**
1. Check Stripe/PayPal transaction (get payment ID)
2. Verify payment completed
3. Admin → Credits → Manual Add (if payment confirmed)
4. Auto-refund if failed

### Subscription Issues
**Symptoms:** Wrong amount charged, didn't renew  
**Fix:**
1. Check current plan: Admin → User → Subscription
2. Review billing history
3. Explain billing cycle (anniversary date)
4. Process refund if error (within 30 days)

---

## Tool Problems (30% of tickets)

### Tool Won't Load
**Symptoms:** Infinite loading, crashes  
**Fix:**
1. Check status.craudiovizai.com
2. Clear cache/cookies
3. Try incognito mode
4. Check Vercel logs for errors
5. If widespread: Create incident

### Export Failed
**Symptoms:** Download fails, corrupted file  
**Fix:**
1. Try different format
2. Regenerate (refund original)
3. Check file size (>100MB may timeout)
4. Test on different browser

### Poor Quality Output
**Symptoms:** AI results disappointing  
**Fix:**
1. Explain AI limitations
2. Offer free regeneration
3. Provide prompt tips
4. Show example outputs
5. Escalate if defective

---

## Technical Issues (20% of tickets)

### Slow Performance
**Symptoms:** Lag, timeouts  
**Fix:**
1. Check user's internet speed
2. Browser up-to-date?
3. Clear cache
4. Close other tabs
5. Check server status (Vercel/Supabase)

### Browser Compatibility
**Symptoms:** Broken layout, buttons not working  
**Fix:**
1. Recommend Chrome (best support)
2. Update browser
3. Disable extensions
4. Check console errors

### Mobile Issues
**Symptoms:** App problems  
**Fix:**
Note: Mobile app Q1 2026
1. Recommend mobile web browser
2. Clear app cache if using beta
3. Reinstall if crashing

---

## Billing & Credits (10% of tickets)

### Refund Request
**Policy:** 30-day money-back (first purchase only)  
**Fix:**
1. Check eligibility (within 30 days?)
2. Verify first purchase
3. If yes: Process immediately
4. If no but legitimate issue: Escalate to manager

**Auto-Refund Triggers:**
- Platform error
- Downtime >15 min
- Incorrect deduction

### Plan Change
**Upgrade:** Immediate, prorated charge  
**Downgrade:** Next billing cycle  
**Fix:**
1. Show current plan details
2. Explain what changes when
3. Process via Admin → Subscriptions

---

## Quick Diagnostics

### Check These First
1. **Service Status:** status.craudiovizai.com
2. **Recent Deployments:** Vercel (new bugs?)
3. **Database:** Supabase (connection issues?)
4. **User's Browser:** Chrome recommended
5. **Error Logs:** Admin Dashboard → System Health

### Get This Info
- Email address (verify identity)
- Browser + version
- Operating system
- Screenshot of error
- Steps to reproduce
- Time/date of issue

---

## Escalation Rules

### Escalate Immediately
- Security breach
- Multiple users same bug
- Payment processing down
- Complete outage
- Fraud suspected
- Legal threat

### Escalate to Senior Support
- Complex technical issues
- Refund outside policy
- Customer wants supervisor
- Account restoration
- Custom requests

### Escalate to Dev Team
- Confirmed bugs
- Database issues
- API problems
- Performance issues

---

## Response Templates

### Opening
> Hi [Name]! Thanks for reaching out. I'll help you with [issue] right away.

### Acknowledging Frustration
> I understand your frustration and apologize for the inconvenience. Let me fix this for you.

### Can't Resolve Immediately
> I've escalated this to our [team]. They'll reach out within [timeframe]. Your ticket: [ID]

### Resolved
> Great! Is there anything else I can help you with today?

---

## Auto-Refund System

**Triggers:**
- Platform error during creation
- Service downtime >15 minutes
- Incorrect credit deduction
- Failed deliverable

**Process:**
1. System detects error
2. Credits refunded automatically
3. Email sent to user
4. No ticket required
5. Logged for review

---

## Common Error Messages

### "Payment Failed"
**Causes:** Insufficient funds, expired card, fraud block  
**Fix:** Try different payment method, verify card details

### "Tool Not Available"
**Causes:** Maintenance, outage, removed  
**Fix:** Check status page, provide ETA, offer alternative

### "Credits Expired"
**Causes:** Free plan (monthly reset), purchased pack expired  
**Fix:** Explain expiration policy, suggest paid plan

### "File Too Large"
**Causes:** Upload exceeds limit  
**Fix:** Compress file, split into parts, use different format

---

## Tips for Success

### Do
- Listen first
- Show empathy
- Provide clear steps
- Follow up
- Document everything

### Don't
- Argue
- Make excuses
- Use jargon
- Overpromise
- Leave unresolved

### Always
- Verify identity
- Test solutions
- Refund when appropriate
- Escalate when needed
- Thank customer

---

**For detailed procedures:** See SUPPORT_PROCEDURES.md  
**For escalation:** See ESCALATION_WORKFLOWS.md

**Questions?** support-training@craudiovizai.com
