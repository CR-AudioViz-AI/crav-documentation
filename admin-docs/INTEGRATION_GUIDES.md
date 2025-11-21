# Integration Guides

**Third-Party Services & APIs**  
**Updated:** November 21, 2025 - 4:59 PM EST

---

## Payment Processing

### Stripe Integration

**Setup:**
1. Create Stripe account
2. Get API keys (Dashboard → Developers → API keys)
3. Add to Vercel: `STRIPE_SECRET_KEY`, `STRIPE_PUBLISHABLE_KEY`
4. Configure webhook: `STRIPE_WEBHOOK_SECRET`

**Webhook URL:** `https://craudiovizai.com/api/webhooks/stripe`

**Events:**
- `payment_intent.succeeded`
- `payment_intent.failed`
- `customer.subscription.created`
- `customer.subscription.updated`
- `customer.subscription.deleted`

**Testing:**
```bash
# Test card
4242 4242 4242 4242
Exp: Any future date
CVC: Any 3 digits
ZIP: Any 5 digits
```

### PayPal Integration

**Setup:**
1. Create PayPal Business account
2. Get credentials (Developer → My Apps)
3. Add to Vercel: `PAYPAL_CLIENT_ID`, `PAYPAL_CLIENT_SECRET`
4. Set mode: `PAYPAL_MODE=production`

**Webhook URL:** `https://craudiovizai.com/api/webhooks/paypal`

**Events:**
- `PAYMENT.SALE.COMPLETED`
- `BILLING.SUBSCRIPTION.CREATED`
- `BILLING.SUBSCRIPTION.CANCELLED`

---

## AI Providers

### OpenAI

**Setup:**
1. Create account at platform.openai.com
2. Add payment method
3. Generate API key
4. Add to Vercel: `OPENAI_API_KEY`

**Models:**
- GPT-4: Most capable
- GPT-3.5-turbo: Fast, cheap

**Rate Limits:**
- Free tier: 3 requests/min
- Pay-as-you-go: 60 requests/min

### Anthropic Claude

**Setup:**
1. Apply at anthropic.com
2. Get approved
3. Generate API key
4. Add to Vercel: `ANTHROPIC_API_KEY`

**Models:**
- Claude 3.5 Sonnet: Best balance
- Claude 3 Opus: Most capable
- Claude 3 Haiku: Fastest

### Google Gemini

**Setup:**
1. Enable Gemini API in Google Cloud
2. Create API key
3. Add to Vercel: `GEMINI_API_KEY`

**Models:**
- Gemini Pro: General purpose
- Gemini Pro Vision: Image understanding

### Perplexity

**Setup:**
1. Sign up at perplexity.ai
2. Get API key
3. Add to Vercel: `PERPLEXITY_API_KEY`

**Use:** Real-time web search

---

## Database

### Supabase

**Setup:**
1. Create project at supabase.com
2. Get credentials from Settings → API
3. Add to Vercel:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY`

**Database:**
- PostgreSQL 15
- Automatic backups
- Built-in auth
- Storage included

**Configuration:**
```sql
-- Enable extensions
create extension if not exists "uuid-ossp";
create extension if not exists "pgcrypto";

-- Run migrations
npm run migrate
```

---

## Hosting & Deployment

### Vercel

**Setup:**
1. Connect GitHub repository
2. Configure build settings:
   - Framework: Next.js
   - Build Command: `npm run build`
   - Output Directory: `.next`
3. Add environment variables
4. Enable preview deployments

**Configuration (vercel.json):**
```json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "installCommand": "npm install",
  "autoAlias": false
}
```

### GitHub

**Setup:**
1. Create organization: CR-AudioViz-AI
2. Add team members
3. Configure branch protection:
   - Require PR reviews
   - Require status checks
   - Restrict force pushes

**Actions:**
- Automatic deployments to Vercel
- Run tests on PR
- Security scanning

---

## Email

### SendGrid

**Setup:**
1. Create account at sendgrid.com
2. Verify sender domain
3. Generate API key
4. Add to Vercel: `SENDGRID_API_KEY`

**Templates:**
- Welcome email
- Password reset
- Credit purchase receipt
- Subscription confirmation

**Sending:**
```typescript
import sgMail from '@sendgrid/mail'
sgMail.setApiKey(process.env.SENDGRID_API_KEY)

await sgMail.send({
  to: user.email,
  from: 'noreply@craudiovizai.com',
  subject: 'Welcome!',
  html: '<p>Welcome to CR AudioViz AI!</p>'
})
```

---

## Storage

### AWS S3 (Optional)

**Setup:**
1. Create S3 bucket
2. Configure CORS
3. Create IAM user with S3 access
4. Add credentials to Vercel

**Use:** Long-term file storage, backups

### Supabase Storage

**Setup:**
- Included with Supabase
- Create buckets in Dashboard → Storage
- Configure public/private access

**Buckets:**
- `avatars`: Public, user profile images
- `projects`: Private, user projects
- `exports`: Private, generated files

---

## Analytics

### Vercel Analytics

**Setup:**
- Automatic with Vercel deployment
- View in Dashboard → Analytics

**Metrics:**
- Page views
- Unique visitors
- Performance (Web Vitals)
- Top pages

### Google Analytics (Optional)

**Setup:**
1. Create GA4 property
2. Get measurement ID
3. Add to Vercel: `NEXT_PUBLIC_GA_MEASUREMENT_ID`

---

## Monitoring

### Sentry (Optional)

**Setup:**
1. Create project at sentry.io
2. Get DSN
3. Add to Vercel: `NEXT_PUBLIC_SENTRY_DSN`
4. Install: `npm install @sentry/nextjs`

**Features:**
- Error tracking
- Performance monitoring
- Release tracking

---

## Testing

### Stripe Test Mode

**Enable:**
- Use test API keys (starts with `sk_test_`)
- Test cards work automatically

**Test Scenarios:**
- Successful payment: 4242 4242 4242 4242
- Declined: 4000 0000 0000 0002
- Insufficient funds: 4000 0000 0000 9995

### API Testing

**Tools:**
- Postman: API development
- Insomnia: REST client
- curl: Command line

**Test Endpoints:**
```bash
# Health check
curl https://craudiovizai.com/api/health

# Authenticated request
curl -H "Authorization: Bearer TOKEN" \
  https://craudiovizai.com/api/user/profile
```

---

## Troubleshooting

### Common Issues

**Stripe webhook not working:**
- Check webhook URL is correct
- Verify webhook secret in Vercel
- Check webhook logs in Stripe Dashboard

**Supabase connection error:**
- Verify URL and keys
- Check network connectivity
- Review connection pool settings

**Vercel deployment failed:**
- Check build logs
- Verify all dependencies installed
- Ensure environment variables set

**Email not sending:**
- Verify SendGrid API key
- Check sender domain verified
- Review email logs

---

## Security Best Practices

**API Keys:**
- Never commit to Git
- Use environment variables
- Rotate every 90 days
- Different keys per environment

**Webhooks:**
- Always verify signatures
- Use HTTPS only
- Implement replay protection
- Log all events

**Database:**
- Use connection pooling
- Enable SSL
- Implement RLS
- Regular backups

---

## Support Contacts

**Stripe:** support@stripe.com  
**PayPal:** developer.paypal.com/support  
**OpenAI:** help.openai.com  
**Anthropic:** support@anthropic.com  
**Supabase:** support@supabase.io  
**Vercel:** support@vercel.com  
**SendGrid:** support@sendgrid.com

---

**Questions?** devops@craudiovizai.com
