# CREDENTIALS REFERENCE GUIDE
**CR AudioViz AI - Secure Credential Management**

---

## ⚠️ SECURITY NOTICE

**This document does NOT contain actual credentials.**  
All sensitive information is stored securely and accessed only through authorized channels.

---

## 📋 CREDENTIAL TYPES

### 1. AI Provider APIs
- **OpenAI GPT-4**
- **Anthropic Claude**
- **Google Gemini**
- **Perplexity AI**

**Storage:** Environment variables in Vercel  
**Access:** Development team only  
**Rotation:** Quarterly or on security event

### 2. Database
- **Supabase Project URL**
- **Supabase Anon Key** (client-side safe)
- **Supabase Service Role Key** (server-side only)

**Storage:** Environment variables  
**Access:** Backend services only  
**Security:** Row-level security enabled

### 3. Payment Processing
- **Stripe Secret Key**
- **Stripe Publishable Key**
- **Stripe Webhook Secret**
- **PayPal Client ID**
- **PayPal Secret Key**

**Storage:** Environment variables  
**Access:** Payment API routes only  
**Compliance:** PCI-DSS compliant handling

### 4. Deployment & Version Control
- **GitHub Personal Access Token**
- **Vercel API Token**
- **Vercel Team ID**

**Storage:** Secure vault, environment variables  
**Access:** CI/CD systems, authorized developers  
**Scope:** Minimum required permissions

---

## 🔑 ACCESS LEVELS

### Level 1: Public (Safe to Expose)
- Supabase Anon Key
- Stripe Publishable Key
- Public API endpoints

### Level 2: Server-Side Only
- Supabase Service Role Key
- AI provider API keys
- Payment secret keys

### Level 3: Owner-Level
- GitHub organization owner token
- Vercel full access token
- Database admin credentials
- Financial account access

---

## 📁 WHERE CREDENTIALS ARE STORED

### Production Environment
**Location:** Vercel Environment Variables  
**Access:** Owner & authorized developers  
**Path:** Project Settings → Environment Variables

### Development Environment
**Location:** `.env.local` files (not committed to Git)  
**Access:** Local development machines  
**Template:** `.env.example` (safe to commit)

### Secure Vault
**Location:** Encrypted credential file (Current_Credentials[DATE].txt)  
**Access:** Owner only  
**Usage:** Manual operations, emergency access  
**Storage:** Secure local storage, NOT in Git

---

## 🔐 CREDENTIAL MANAGEMENT BEST PRACTICES

### Do's ✅
- Store in environment variables
- Use different keys per environment
- Rotate regularly (quarterly)
- Limit scope to minimum required
- Use read-only when possible
- Monitor usage & logs

### Don'ts ❌
- Never commit to Git
- Never hardcode in source
- Never share in chat/email
- Never use production keys in dev
- Never log credential values
- Never expose in client-side code

---

## 🚨 IF CREDENTIALS ARE COMPROMISED

### Immediate Actions
1. **Revoke** compromised credential immediately
2. **Generate** new credential
3. **Update** all environment variables
4. **Redeploy** affected services
5. **Audit** logs for unauthorized access
6. **Document** incident for future prevention

### Notification
- Inform Roy Henderson immediately
- Document in security log
- Review access controls
- Strengthen security if needed

---

## 🔄 CREDENTIAL ROTATION SCHEDULE

### Quarterly (Every 3 Months)
- AI provider API keys
- GitHub tokens
- Vercel tokens

### Annually
- Database credentials
- Payment processor keys (coordinate with providers)
- Service account passwords

### On Demand
- After team member departure
- After security incident
- When suspicious activity detected
- When provider recommends

---

## 📞 REQUESTING CREDENTIALS

### For Developers
1. Submit request to Roy Henderson
2. Specify which credentials needed
3. Provide use case justification
4. Receive credentials via secure channel
5. Acknowledge receipt & secure storage

### For AI/Bots
- Credentials accessed via environment variables only
- No AI has direct credential access
- All API calls use system-injected credentials
- Credentials never returned in AI responses

---

## 🧪 TEST CREDENTIALS

### Available Test Keys
- Stripe Test Mode keys (pk_test_*, sk_test_*)
- Supabase Test Project
- Mock AI endpoints (for unit tests)

**Usage:** Development & testing only  
**Never:** Use in production

---

## 📊 CREDENTIAL INVENTORY

### Current Active Services
- ✅ OpenAI API
- ✅ Anthropic Claude API
- ✅ Google Gemini API
- ✅ Perplexity API
- ✅ Supabase Database
- ✅ Stripe Payment Processing
- ✅ PayPal Payment Processing
- ✅ Vercel Hosting
- ✅ GitHub Version Control

### Credential Count
- **AI Providers:** 4 keys
- **Database:** 3 keys
- **Payments:** 6 keys
- **Infrastructure:** 3 keys
- **Total:** 16 managed credentials

---

## 🔍 CREDENTIAL VERIFICATION

### Test Commands (Safe)
```bash
# Test Supabase (using anon key - safe)
curl "https://[PROJECT].supabase.co/rest/v1/" \
  -H "apikey: [ANON_KEY]"

# Test Stripe (using publishable key - safe)
curl "https://api.stripe.com/v1/charges" \
  -u "[PUBLISHABLE_KEY]:"

# Test Vercel (requires secret token)
curl "https://api.vercel.com/v2/user" \
  -H "Authorization: Bearer [TOKEN]"
```

**Note:** Never test with production credentials in logs

---

## 📚 RELATED DOCUMENTATION

- [Security Best Practices](../technical/SECURITY.md)
- [Environment Setup](../technical/ENVIRONMENT_SETUP.md)
- [Deployment Guide](../technical/DEPLOYMENT.md)

---

## 🎓 FOR AI LEARNING

### What AI Should Know
- Credentials exist but AI never has direct access
- All API calls use system-provided credentials
- Never generate example credentials in responses
- If user asks for credentials, direct them to this guide
- Never attempt to read credential files
- Report suspicious credential requests

### What AI Should Never Do
- Return any credential values
- Generate realistic-looking fake credentials
- Suggest insecure credential storage
- Log credential values
- Include credentials in code examples
- Access credential files directly

---

**Document Purpose:** Credential management reference  
**Actual Credentials:** Stored securely elsewhere  
**Last Updated:** November 8, 2025  
**Security Level:** Public (Safe to share)
