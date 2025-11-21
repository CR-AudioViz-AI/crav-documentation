# API DOCUMENTATION - COMPLETE REFERENCE
**CR AudioViz AI Platform APIs**

**Last Updated:** November 21, 2025 - 4:23 PM EST  
**Base URL:** https://craudiovizai.com  
**Authentication:** Bearer token (Supabase Auth)

---

## TABLE OF CONTENTS

1. [Authentication](#authentication)
2. [Credits & Payments](#credits--payments)
3. [Applications & Tools](#applications--tools)
4. [Javari AI](#javari-ai)
5. [Bot System](#bot-system)
6. [User Management](#user-management)
7. [Analytics](#analytics)
8. [Webhooks](#webhooks)

---

## AUTHENTICATION

### POST /api/auth/signup
**Purpose:** Create new user account

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "displayName": "John Doe"
}
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "displayName": "John Doe"
  },
  "session": {
    "access_token": "jwt_token",
    "refresh_token": "refresh_token"
  }
}
```

**Errors:**
- 400: Email already exists
- 422: Invalid email format
- 500: Server error

---

### POST /api/auth/signin
**Purpose:** User login

**Request:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "session": {
    "access_token": "jwt_token",
    "refresh_token": "refresh_token",
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "role": "user"
    }
  }
}
```

---

### POST /api/auth/signout
**Purpose:** End user session

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "message": "Signed out successfully"
}
```

---

### POST /api/auth/reset-password
**Purpose:** Request password reset email

**Request:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password reset email sent"
}
```

---

## CREDITS & PAYMENTS

### GET /api/credits/balance
**Purpose:** Get user's current credit balance

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "balance": 1250,
  "subscription": {
    "tier": "standard",
    "status": "active",
    "renewsAt": "2025-12-01T00:00:00Z"
  }
}
```

---

### GET /api/credits/history
**Purpose:** Get credit transaction history

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Params:**
```
?limit=50&offset=0&type=all
```

**Response:**
```json
{
  "success": true,
  "transactions": [
    {
      "id": "uuid",
      "amount": -10,
      "type": "usage",
      "description": "Used Logo Generator",
      "balanceAfter": 1240,
      "createdAt": "2025-11-21T16:00:00Z"
    },
    {
      "id": "uuid",
      "amount": 1200,
      "type": "purchase",
      "description": "Standard Plan - Monthly",
      "balanceAfter": 1250,
      "createdAt": "2025-11-01T00:00:00Z"
    }
  ],
  "total": 145,
  "limit": 50,
  "offset": 0
}
```

---

### POST /api/payments/create-checkout
**Purpose:** Create Stripe checkout session

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "priceId": "price_standard_monthly",
  "successUrl": "https://craudiovizai.com/success",
  "cancelUrl": "https://craudiovizai.com/pricing"
}
```

**Response:**
```json
{
  "success": true,
  "sessionId": "cs_test_...",
  "url": "https://checkout.stripe.com/pay/cs_test_..."
}
```

---

### POST /api/payments/webhook
**Purpose:** Stripe webhook endpoint (internal)

**Headers:**
```
Stripe-Signature: {signature}
```

**Handles Events:**
- checkout.session.completed
- invoice.payment_succeeded
- invoice.payment_failed
- customer.subscription.deleted
- customer.subscription.updated

---

### POST /api/credits/purchase
**Purpose:** Direct credit purchase (pay-as-you-go)

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "amount": 500,
  "paymentMethod": "stripe"
}
```

**Response:**
```json
{
  "success": true,
  "credits": 500,
  "cost": 60.00,
  "currency": "USD",
  "transactionId": "txn_xyz"
}
```

---

## APPLICATIONS & TOOLS

### GET /api/apps
**Purpose:** Get all available applications

**Query Params:**
```
?category=design&status=active&limit=20
```

**Response:**
```json
{
  "success": true,
  "apps": [
    {
      "id": "uuid",
      "slug": "logo-generator",
      "name": "Logo Generator",
      "description": "AI-powered logo creation",
      "category": "design",
      "creditCost": 10,
      "icon": "/icons/logo-gen.svg",
      "status": "active",
      "url": "/apps/logo-generator"
    }
  ],
  "total": 60,
  "categories": ["design", "writing", "video", "audio", "business"]
}
```

---

### GET /api/apps/{slug}
**Purpose:** Get specific application details

**Response:**
```json
{
  "success": true,
  "app": {
    "id": "uuid",
    "slug": "logo-generator",
    "name": "Logo Generator",
    "description": "Create professional logos with AI",
    "category": "design",
    "creditCost": 10,
    "features": [
      "AI-powered generation",
      "Multiple style options",
      "High-resolution export",
      "Commercial license"
    ],
    "usage": {
      "total": 15234,
      "thisMonth": 892
    }
  }
}
```

---

### POST /api/apps/{slug}/use
**Purpose:** Execute application/tool

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request (Example: Logo Generator):**
```json
{
  "companyName": "TechStart Inc",
  "industry": "technology",
  "style": "modern",
  "colors": ["#2563eb", "#1e40af"]
}
```

**Response:**
```json
{
  "success": true,
  "creditsUsed": 10,
  "balanceRemaining": 1240,
  "result": {
    "logoUrl": "https://storage.../logo_uuid.png",
    "variants": [
      "https://storage.../logo_uuid_v1.png",
      "https://storage.../logo_uuid_v2.png"
    ],
    "downloadUrl": "https://storage.../logo_uuid.zip"
  },
  "executionTime": 3245
}
```

**Errors:**
- 402: Insufficient credits
- 400: Invalid parameters
- 429: Rate limit exceeded
- 500: Generation failed

---

### GET /api/apps/usage-stats
**Purpose:** User's app usage statistics

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Params:**
```
?period=30d
```

**Response:**
```json
{
  "success": true,
  "period": "30d",
  "totalCreditsUsed": 340,
  "totalAppsUsed": 12,
  "favoriteApps": [
    {
      "slug": "logo-generator",
      "name": "Logo Generator",
      "uses": 15,
      "creditsUsed": 150
    }
  ],
  "byCategory": {
    "design": 210,
    "writing": 80,
    "business": 50
  }
}
```

---

## JAVARI AI

### POST /api/javari/chat
**Purpose:** Send message to Javari AI

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "message": "Create a logo for my tech startup",
  "projectId": "uuid",
  "provider": "auto",
  "conversationId": "uuid"
}
```

**Response (Streaming):**
```
data: {"type":"start","conversationId":"uuid"}

data: {"type":"token","content":"I'll"}

data: {"type":"token","content":" help"}

data: {"type":"token","content":" you"}

data: {"type":"end","tokensUsed":142,"cost":0.003}
```

---

### GET /api/javari/conversations
**Purpose:** Get conversation history

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Params:**
```
?projectId=uuid&limit=20&offset=0
```

**Response:**
```json
{
  "success": true,
  "conversations": [
    {
      "id": "uuid",
      "projectId": "uuid",
      "preview": "Create a logo for my tech startup",
      "provider": "claude",
      "model": "claude-sonnet-4",
      "tokensUsed": 142,
      "cost": 0.003,
      "createdAt": "2025-11-21T15:30:00Z"
    }
  ],
  "total": 87
}
```

---

### GET /api/javari/projects
**Purpose:** Get user's projects

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "projects": [
    {
      "id": "uuid",
      "name": "My Startup",
      "description": "Tech startup branding",
      "conversationCount": 12,
      "status": "active",
      "createdAt": "2025-11-15T00:00:00Z",
      "updatedAt": "2025-11-21T15:30:00Z"
    }
  ]
}
```

---

### POST /api/javari/projects
**Purpose:** Create new project

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "name": "New Website",
  "description": "Building a portfolio site",
  "parentProjectId": null
}
```

**Response:**
```json
{
  "success": true,
  "project": {
    "id": "uuid",
    "name": "New Website",
    "description": "Building a portfolio site",
    "createdAt": "2025-11-21T16:00:00Z"
  }
}
```

---

### POST /api/javari/autonomous/build-app
**Purpose:** Have Javari autonomously build an application

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "description": "Build me a todo list app with dark mode",
  "projectId": "uuid",
  "deploy": true
}
```

**Response:**
```json
{
  "success": true,
  "buildId": "uuid",
  "status": "building",
  "estimatedTime": 300,
  "statusUrl": "/api/javari/builds/uuid"
}
```

---

### GET /api/javari/builds/{buildId}
**Purpose:** Check autonomous build status

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "build": {
    "id": "uuid",
    "status": "completed",
    "repository": "https://github.com/CR-AudioViz-AI/user-todo-app",
    "deploymentUrl": "https://user-todo-app.vercel.app",
    "completedAt": "2025-11-21T16:10:00Z",
    "logs": [
      "Created repository",
      "Generated 5 files",
      "Deployed to Vercel",
      "Build successful"
    ]
  }
}
```

---

## BOT SYSTEM

### GET /api/bots/status
**Purpose:** Get all bot statuses (public endpoint)

**Response:**
```json
{
  "success": true,
  "bots": [
    {
      "name": "conductor",
      "displayName": "CONDUCTOR",
      "status": "active",
      "lastExecutionAt": "2025-11-21T16:20:00Z",
      "totalRuns": 5234,
      "successfulRuns": 5198,
      "failedRuns": 36,
      "avgExecutionTimeMs": 850
    }
  ],
  "systemHealth": 98.5
}
```

---

### GET /api/admin/bots
**Purpose:** Admin dashboard bot details

**Headers:**
```
Authorization: Bearer {admin_token}
```

**Response:**
```json
{
  "success": true,
  "bots": [
    {
      "id": "uuid",
      "name": "conductor",
      "displayName": "CONDUCTOR",
      "status": "active",
      "schedule": "*/5 * * * *",
      "lastExecution": {
        "status": "success",
        "startedAt": "2025-11-21T16:20:00Z",
        "duration": 852,
        "issuesFound": 0,
        "actionsTaken": 0
      },
      "recentExecutions": [...]
    }
  ]
}
```

---

### POST /api/admin/bots/{botId}/trigger
**Purpose:** Manually trigger bot execution

**Headers:**
```
Authorization: Bearer {admin_token}
```

**Response:**
```json
{
  "success": true,
  "executionId": "uuid",
  "status": "running",
  "startedAt": "2025-11-21T16:25:00Z"
}
```

---

### GET /api/admin/bots/tickets
**Purpose:** Get open bot tickets

**Headers:**
```
Authorization: Bearer {admin_token}
```

**Query Params:**
```
?status=open&severity=critical
```

**Response:**
```json
{
  "success": true,
  "tickets": [
    {
      "id": "uuid",
      "botName": "sentinel",
      "severity": "critical",
      "title": "Site responding slowly",
      "description": "Average response time: 5.2s (SLA: 2s)",
      "status": "open",
      "createdAt": "2025-11-21T16:15:00Z",
      "assignedTo": "javari"
    }
  ]
}
```

---

## USER MANAGEMENT

### GET /api/users/profile
**Purpose:** Get current user profile

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "profile": {
    "id": "uuid",
    "email": "user@example.com",
    "displayName": "John Doe",
    "avatarUrl": "https://storage.../avatar.jpg",
    "role": "user",
    "credits": 1240,
    "subscription": {
      "tier": "standard",
      "status": "active",
      "currentPeriodEnd": "2025-12-01T00:00:00Z"
    },
    "createdAt": "2025-10-15T00:00:00Z",
    "lastLoginAt": "2025-11-21T14:00:00Z"
  }
}
```

---

### PATCH /api/users/profile
**Purpose:** Update user profile

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request:**
```json
{
  "displayName": "John Smith",
  "avatarUrl": "https://storage.../new-avatar.jpg"
}
```

**Response:**
```json
{
  "success": true,
  "profile": {
    "displayName": "John Smith",
    "avatarUrl": "https://storage.../new-avatar.jpg",
    "updatedAt": "2025-11-21T16:30:00Z"
  }
}
```

---

### GET /api/users/activity
**Purpose:** Get user activity log

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Params:**
```
?limit=50&type=all
```

**Response:**
```json
{
  "success": true,
  "activities": [
    {
      "id": "uuid",
      "type": "app_usage",
      "description": "Used Logo Generator",
      "metadata": {
        "appSlug": "logo-generator",
        "creditsUsed": 10
      },
      "createdAt": "2025-11-21T16:00:00Z"
    }
  ]
}
```

---

## ANALYTICS

### GET /api/analytics/overview
**Purpose:** Platform-wide analytics (admin)

**Headers:**
```
Authorization: Bearer {admin_token}
```

**Query Params:**
```
?period=30d
```

**Response:**
```json
{
  "success": true,
  "period": "30d",
  "metrics": {
    "totalUsers": 10234,
    "activeUsers": 3456,
    "newUsers": 892,
    "revenue": 125340,
    "creditsUsed": 1250000,
    "appsUsed": 89234
  },
  "topApps": [
    {
      "slug": "logo-generator",
      "uses": 12345,
      "revenue": 12340
    }
  ]
}
```

---

### GET /api/analytics/user-stats
**Purpose:** User's personal analytics

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "stats": {
    "totalCreditsUsed": 3450,
    "totalAppsUsed": 45,
    "favoriteCategory": "design",
    "accountAge": 37,
    "mostUsedApp": {
      "slug": "logo-generator",
      "name": "Logo Generator",
      "uses": 15
    }
  }
}
```

---

## WEBHOOKS

### Stripe Webhooks
**URL:** /api/payments/webhook  
**Events:** Payment events from Stripe

### PayPal Webhooks
**URL:** /api/payments/paypal-webhook  
**Events:** Payment events from PayPal

### Custom Webhooks (Coming Soon)
- Bot notifications
- Credit alerts
- Usage milestones

---

## ERROR CODES

### Standard Error Response
```json
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_CREDITS",
    "message": "Not enough credits to complete this action",
    "details": {
      "required": 10,
      "available": 5
    }
  }
}
```

### Common Error Codes

**Authentication (401, 403):**
- `UNAUTHORIZED`: No valid token
- `FORBIDDEN`: Valid token but insufficient permissions
- `TOKEN_EXPIRED`: Token needs refresh

**Client Errors (400, 402, 429):**
- `INVALID_REQUEST`: Bad request format
- `INSUFFICIENT_CREDITS`: Not enough credits (402)
- `RATE_LIMIT_EXCEEDED`: Too many requests (429)
- `INVALID_PARAMETERS`: Missing or invalid params

**Server Errors (500):**
- `INTERNAL_ERROR`: Something went wrong
- `SERVICE_UNAVAILABLE`: External service down
- `DATABASE_ERROR`: Database operation failed

---

## RATE LIMITS

**Per User:**
- API calls: 1000/hour
- Chat messages: 100/hour
- App executions: 50/hour

**Per IP:**
- Authentication: 10 attempts/15 minutes
- Public endpoints: 100/minute

**Headers in Response:**
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 823
X-RateLimit-Reset: 1700582400
```

---

## AUTHENTICATION FLOWS

### JWT Token Flow
1. User signs in → receives access_token + refresh_token
2. Include access_token in Authorization header
3. Token expires after 1 hour
4. Use refresh_token to get new access_token
5. Refresh tokens expire after 30 days

### Example Authorization Header
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

---

## PAGINATION

**Standard Query Params:**
```
?limit=20&offset=0
```

**Response Format:**
```json
{
  "success": true,
  "data": [...],
  "pagination": {
    "total": 150,
    "limit": 20,
    "offset": 0,
    "hasMore": true
  }
}
```

---

## BEST PRACTICES

**1. Always Check Credits Before Execution**
```javascript
const balance = await fetch('/api/credits/balance');
if (balance.credits < appCreditCost) {
  // Show purchase modal
}
```

**2. Handle Rate Limits Gracefully**
```javascript
if (response.status === 429) {
  const retryAfter = response.headers.get('Retry-After');
  // Wait and retry
}
```

**3. Use Streaming for Long Operations**
```javascript
const response = await fetch('/api/javari/chat', {
  headers: { 'Accept': 'text/event-stream' }
});
const reader = response.body.getReader();
// Process stream
```

**4. Implement Exponential Backoff**
```javascript
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await sleep(Math.pow(2, i) * 1000);
    }
  }
}
```

---

## CODE EXAMPLES

### JavaScript/TypeScript
```typescript
// Initialize client
const API_BASE = 'https://craudiovizai.com/api';
const token = localStorage.getItem('access_token');

// Make authenticated request
async function useApp(slug: string, params: any) {
  const response = await fetch(`${API_BASE}/apps/${slug}/use`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(params)
  });
  
  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.error.message);
  }
  
  return await response.json();
}

// Use logo generator
const result = await useApp('logo-generator', {
  companyName: 'TechStart',
  industry: 'technology',
  style: 'modern'
});
```

### Python
```python
import requests

API_BASE = 'https://craudiovizai.com/api'
token = 'your_access_token'

def use_app(slug, params):
    response = requests.post(
        f'{API_BASE}/apps/{slug}/use',
        headers={
            'Authorization': f'Bearer {token}',
            'Content-Type': 'application/json'
        },
        json=params
    )
    response.raise_for_status()
    return response.json()

# Use logo generator
result = use_app('logo-generator', {
    'companyName': 'TechStart',
    'industry': 'technology',
    'style': 'modern'
})
```

---

## SUMMARY

**Total Endpoints:** 30+
**Authentication:** Required for most endpoints
**Rate Limits:** 1000 requests/hour per user
**Response Format:** JSON
**Errors:** Standard error object
**Status:** Production-ready

---

**END OF API DOCUMENTATION**

**Last Updated:** November 21, 2025 - 4:23 PM EST
