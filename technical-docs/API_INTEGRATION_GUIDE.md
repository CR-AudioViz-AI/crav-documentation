# CR AudioViz AI - API Integration Guide

**Version:** 1.0  
**Last Updated:** November 21, 2025 - 11:54 AM EST  
**Base URL:** https://api.craudiovizai.com  
**Audience:** Developers, Integration Partners

---

## Quick Start

### Authentication

All API requests require authentication via Bearer token:

```bash
curl https://api.craudiovizai.com/v1/endpoint \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json"
```

**Get API Key:**
1. Log in to https://craudiovizai.com
2. Go to Settings → API Keys
3. Click "Generate New Key"
4. Copy and store securely

---

## Core Endpoints

### User Management

**Get User Profile:**
```bash
GET /v1/user/profile
```

**Response:**
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "credits": 500,
  "plan": "pro",
  "created_at": "2025-11-01T00:00:00Z"
}
```

**Update Profile:**
```bash
PATCH /v1/user/profile
Content-Type: application/json

{
  "name": "John Doe",
  "avatar_url": "https://..."
}
```

### Credit System

**Check Balance:**
```bash
GET /v1/credits/balance
```

**Response:**
```json
{
  "available": 500,
  "reserved": 50,
  "total": 550,
  "plan": "pro",
  "never_expires": true
}
```

**Purchase Credits:**
```bash
POST /v1/credits/purchase
Content-Type: application/json

{
  "amount": 1000,
  "payment_method": "stripe_pm_xxx"
}
```

### Projects

**List Projects:**
```bash
GET /v1/projects?limit=20&offset=0
```

**Create Project:**
```bash
POST /v1/projects
Content-Type: application/json

{
  "name": "My Project",
  "description": "Project description",
  "privacy": "private"
}
```

**Get Project:**
```bash
GET /v1/projects/{project_id}
```

### AI Requests

**Javari Chat:**
```bash
POST /v1/javari/chat
Content-Type: application/json

{
  "message": "Help me create a logo",
  "context": {
    "project_id": "proj_123",
    "user_preferences": {...}
  }
}
```

**Response (Streaming):**
```json
data: {"type":"token","content":"I"}
data: {"type":"token","content":" can"}
data: {"type":"token","content":" help"}
data: {"type":"done","cost":0.02}
```

**Tool Execution:**
```bash
POST /v1/tools/{tool_name}/execute
Content-Type: application/json

{
  "input": {
    "prompt": "Modern tech startup logo",
    "style": "minimal",
    "colors": ["#0066FF", "#FFFFFF"]
  },
  "project_id": "proj_123"
}
```

---

## Market Oracle API

### Stock Analysis

**Get AI Stock Picks:**
```bash
GET /v1/market/picks?timeframe=1w&limit=10
```

**Response:**
```json
{
  "picks": [
    {
      "symbol": "AAPL",
      "ai_provider": "claude",
      "sentiment": "bullish",
      "target_price": 225.00,
      "current_price": 220.50,
      "confidence": 0.85,
      "reasoning": "Strong fundamentals...",
      "created_at": "2025-11-21T10:00:00Z"
    }
  ],
  "total": 101,
  "page": 1
}
```

**Get Real-time Quote:**
```bash
GET /v1/market/quote/{symbol}
```

**Response:**
```json
{
  "symbol": "AAPL",
  "price": 220.50,
  "change": 2.30,
  "change_percent": 1.05,
  "volume": 52431890,
  "market_cap": 3420000000000,
  "updated_at": "2025-11-21T16:00:00Z"
}
```

### News & Sentiment

**Get Market News:**
```bash
GET /v1/market/news?symbol=AAPL&limit=10
```

**Sentiment Analysis:**
```bash
POST /v1/market/sentiment
Content-Type: application/json

{
  "symbol": "AAPL",
  "timeframe": "1d"
}
```

---

## Webhooks

### Setup

**Create Webhook:**
```bash
POST /v1/webhooks
Content-Type: application/json

{
  "url": "https://yourapp.com/webhook",
  "events": ["project.created", "tool.completed", "credit.low"],
  "secret": "your_webhook_secret"
}
```

### Events

**project.created:**
```json
{
  "event": "project.created",
  "data": {
    "project_id": "proj_123",
    "name": "My Project",
    "user_id": "user_123"
  },
  "timestamp": "2025-11-21T12:00:00Z"
}
```

**tool.completed:**
```json
{
  "event": "tool.completed",
  "data": {
    "tool_name": "logo-maker",
    "project_id": "proj_123",
    "output_url": "https://...",
    "credits_used": 25
  },
  "timestamp": "2025-11-21T12:05:00Z"
}
```

**credit.low:**
```json
{
  "event": "credit.low",
  "data": {
    "user_id": "user_123",
    "current_balance": 50,
    "threshold": 100
  },
  "timestamp": "2025-11-21T12:10:00Z"
}
```

### Verification

Webhooks include `X-Signature` header with HMAC-SHA256 signature:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hmac = crypto.createHmac('sha256', secret);
  const digest = hmac.update(payload).digest('hex');
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(digest)
  );
}
```

---

## Rate Limits

### Limits by Plan

**Free:**
- 100 requests/hour
- 1,000 requests/day

**Starter:**
- 500 requests/hour
- 10,000 requests/day

**Pro:**
- 2,000 requests/hour
- 50,000 requests/day

**Enterprise:**
- Custom limits

### Headers

Every response includes rate limit info:

```
X-RateLimit-Limit: 2000
X-RateLimit-Remaining: 1995
X-RateLimit-Reset: 1637520000
```

### Handling Rate Limits

**Status Code:** 429 Too Many Requests

**Response:**
```json
{
  "error": "rate_limit_exceeded",
  "message": "You have exceeded your rate limit",
  "retry_after": 3600
}
```

---

## Error Handling

### Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `429` - Rate Limit Exceeded
- `500` - Internal Server Error
- `503` - Service Unavailable

### Error Response Format

```json
{
  "error": "error_code",
  "message": "Human-readable message",
  "details": {
    "field": "specific_field",
    "reason": "validation_failed"
  },
  "request_id": "req_123",
  "timestamp": "2025-11-21T12:00:00Z"
}
```

### Common Errors

**Invalid API Key:**
```json
{
  "error": "invalid_api_key",
  "message": "The API key provided is invalid or expired"
}
```

**Insufficient Credits:**
```json
{
  "error": "insufficient_credits",
  "message": "You need 25 credits but have 10",
  "details": {
    "required": 25,
    "available": 10
  }
}
```

---

## SDKs

### Node.js

```javascript
npm install @craudiovizai/node-sdk

const CRAudioViz = require('@craudiovizai/node-sdk');
const client = new CRAudioViz('your_api_key');

// Chat with Javari
const response = await client.javari.chat({
  message: 'Help me create a logo'
});

// Execute tool
const result = await client.tools.execute('logo-maker', {
  prompt: 'Modern tech startup logo',
  style: 'minimal'
});

// Check credits
const balance = await client.credits.getBalance();
```

### Python

```python
pip install craudiovizai

from craudiovizai import CRAudioViz

client = CRAudioViz(api_key='your_api_key')

# Chat with Javari
response = client.javari.chat(
    message='Help me create a logo'
)

# Execute tool
result = client.tools.execute('logo-maker', {
    'prompt': 'Modern tech startup logo',
    'style': 'minimal'
})

# Check credits
balance = client.credits.get_balance()
```

### cURL Examples

**Logo Generation:**
```bash
curl -X POST https://api.craudiovizai.com/v1/tools/logo-maker/execute \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "prompt": "Modern tech startup logo",
      "style": "minimal",
      "colors": ["#0066FF", "#FFFFFF"]
    }
  }'
```

---

## White-Label API

### Custom Branding

**Configure White-Label:**
```bash
POST /v1/white-label/config
Content-Type: application/json

{
  "company_name": "Your Company",
  "logo_url": "https://...",
  "primary_color": "#0066FF",
  "domain": "yourdomain.com"
}
```

### Embedded Tools

**Generate Embed Code:**
```bash
GET /v1/tools/{tool_name}/embed
```

**Response:**
```html
<iframe 
  src="https://embed.craudiovizai.com/logo-maker?key=YOUR_KEY"
  width="100%"
  height="600px"
  frameborder="0"
></iframe>
```

---

## Testing

### Sandbox Environment

**Base URL:** https://sandbox-api.craudiovizai.com

- Test API keys start with `test_`
- No charges for credit usage
- Reset daily at midnight UTC
- Full feature parity with production

### Test API Key

Get test key from: https://dashboard.craudiovizai.com/settings/test-keys

---

## Best Practices

### Security

1. **Never expose API keys** in client-side code
2. **Rotate keys** every 90 days
3. **Use environment variables** for key storage
4. **Implement webhook signature verification**
5. **Monitor API usage** for anomalies

### Performance

1. **Cache responses** when appropriate (respect Cache-Control)
2. **Use webhooks** instead of polling
3. **Implement exponential backoff** for retries
4. **Batch requests** when possible
5. **Monitor rate limits** proactively

### Error Handling

```javascript
try {
  const result = await client.tools.execute('logo-maker', input);
} catch (error) {
  if (error.code === 'insufficient_credits') {
    // Prompt user to purchase credits
  } else if (error.code === 'rate_limit_exceeded') {
    // Wait and retry
    await sleep(error.retry_after * 1000);
  } else {
    // Log and notify
    logger.error('API error:', error);
  }
}
```

---

## Support

**Documentation:** https://docs.craudiovizai.com  
**API Status:** https://status.craudiovizai.com  
**Community:** https://community.craudiovizai.com  
**Email:** api@craudiovizai.com

---

**END OF API INTEGRATION GUIDE**
