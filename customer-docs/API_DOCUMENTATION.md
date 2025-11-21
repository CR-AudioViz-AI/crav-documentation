# API Documentation

**Developer Reference Guide**  
**API Version:** v1  
**Updated:** November 21, 2025 - 11:52 AM EST

---

## Getting Started

### Base URL
```
https://api.craudiovizai.com/v1
```

### Authentication
All requests require an API key in the header:
```
Authorization: Bearer YOUR_API_KEY
```

### Get Your API Key
1. Login to dashboard
2. Profile → API Keys
3. Click "Generate New Key"
4. Copy and secure your key

⚠️ **Security:** Never commit API keys to Git or share publicly

---

## Rate Limits

| Plan | Requests/Hour | Requests/Day |
|------|---------------|--------------|
| Free | 100 | 1,000 |
| Starter | 500 | 10,000 |
| Pro | 2,000 | 50,000 |
| Business | 10,000 | 250,000 |
| Enterprise | Unlimited | Unlimited |

**Headers returned:**
```
X-RateLimit-Limit: 2000
X-RateLimit-Remaining: 1999
X-RateLimit-Reset: 1637596800
```

---

## Authentication

### API Key Authentication
```bash
curl https://api.craudiovizai.com/v1/user/profile \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Response
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "plan": "pro",
  "credits": 1500
}
```

---

## Credits

### Check Balance
```bash
GET /credits/balance
```

**Response:**
```json
{
  "balance": 1500,
  "plan": "pro",
  "monthly_allowance": 2000,
  "expires_at": null
}
```

### Purchase Credits
```bash
POST /credits/purchase
Content-Type: application/json

{
  "amount": 500,
  "payment_method": "stripe_token_xyz"
}
```

**Response:**
```json
{
  "transaction_id": "txn_123",
  "credits_added": 500,
  "new_balance": 2000,
  "cost": "$40.00"
}
```

### Transaction History
```bash
GET /credits/transactions?limit=50&offset=0
```

**Response:**
```json
{
  "transactions": [
    {
      "id": "txn_123",
      "type": "purchase",
      "amount": 500,
      "balance_after": 2000,
      "created_at": "2025-11-21T11:52:00Z"
    }
  ],
  "total": 150,
  "has_more": true
}
```

---

## Projects

### Create Project
```bash
POST /projects
Content-Type: application/json

{
  "name": "My New Project",
  "description": "Project description",
  "tags": ["logo", "branding"]
}
```

**Response:**
```json
{
  "id": "proj_123",
  "name": "My New Project",
  "created_at": "2025-11-21T11:52:00Z",
  "status": "active"
}
```

### List Projects
```bash
GET /projects?limit=20&offset=0&sort=created_at&order=desc
```

**Query Parameters:**
- `limit` (int): Results per page (max 100)
- `offset` (int): Pagination offset
- `sort` (string): Field to sort by
- `order` (string): asc or desc
- `tags` (array): Filter by tags

**Response:**
```json
{
  "projects": [
    {
      "id": "proj_123",
      "name": "My New Project",
      "items_count": 5,
      "created_at": "2025-11-21T11:52:00Z"
    }
  ],
  "total": 42,
  "has_more": false
}
```

### Get Project
```bash
GET /projects/{project_id}
```

**Response:**
```json
{
  "id": "proj_123",
  "name": "My New Project",
  "description": "Project description",
  "items": [
    {
      "id": "item_456",
      "type": "logo",
      "name": "Logo v1",
      "url": "https://cdn.craudiovizai.com/..."
    }
  ],
  "tags": ["logo", "branding"],
  "created_at": "2025-11-21T11:52:00Z",
  "updated_at": "2025-11-21T12:00:00Z"
}
```

### Update Project
```bash
PATCH /projects/{project_id}
Content-Type: application/json

{
  "name": "Updated Name",
  "tags": ["logo", "branding", "2025"]
}
```

### Delete Project
```bash
DELETE /projects/{project_id}
```

**Response:**
```json
{
  "deleted": true,
  "id": "proj_123"
}
```

---

## Tools

### List Available Tools
```bash
GET /tools
```

**Response:**
```json
{
  "tools": [
    {
      "id": "logo_maker",
      "name": "Logo Maker",
      "category": "design",
      "credits_per_use": 10,
      "description": "Create professional logos",
      "input_schema": {...}
    }
  ]
}
```

### Use Tool
```bash
POST /tools/{tool_id}/execute
Content-Type: application/json

{
  "project_id": "proj_123",
  "inputs": {
    "text": "My Company",
    "style": "modern",
    "colors": ["#FF5733", "#3498db"]
  }
}
```

**Response:**
```json
{
  "job_id": "job_789",
  "status": "processing",
  "estimated_time": 30
}
```

### Check Job Status
```bash
GET /jobs/{job_id}
```

**Response (Processing):**
```json
{
  "job_id": "job_789",
  "status": "processing",
  "progress": 45,
  "message": "Generating design..."
}
```

**Response (Complete):**
```json
{
  "job_id": "job_789",
  "status": "completed",
  "result": {
    "item_id": "item_456",
    "url": "https://cdn.craudiovizai.com/...",
    "formats": ["png", "svg", "pdf"]
  },
  "credits_used": 10,
  "completed_at": "2025-11-21T12:05:00Z"
}
```

---

## AI Assistant (Javari)

### Send Message
```bash
POST /ai/chat
Content-Type: application/json

{
  "message": "Create a logo for my coffee shop",
  "context": {
    "project_id": "proj_123"
  }
}
```

**Response:**
```json
{
  "reply": "I'll help you create a coffee shop logo. What's the name of your shop?",
  "suggestions": [
    "Provide shop name",
    "Choose color scheme",
    "Select style (modern, vintage, minimalist)"
  ],
  "chat_id": "chat_123"
}
```

### Continue Conversation
```bash
POST /ai/chat/{chat_id}/message
Content-Type: application/json

{
  "message": "The shop is called 'Bean There'"
}
```

### Chat History
```bash
GET /ai/chat/{chat_id}/history
```

---

## Files

### Upload File
```bash
POST /files/upload
Content-Type: multipart/form-data

file: [binary data]
project_id: proj_123
```

**Response:**
```json
{
  "file_id": "file_999",
  "url": "https://cdn.craudiovizai.com/...",
  "size": 1024567,
  "type": "image/png"
}
```

### Download File
```bash
GET /files/{file_id}/download
```

Returns file binary with appropriate headers.

### Delete File
```bash
DELETE /files/{file_id}
```

---

## Webhooks

### Configure Webhook
```bash
POST /webhooks
Content-Type: application/json

{
  "url": "https://yoursite.com/webhook",
  "events": ["job.completed", "credits.low"],
  "secret": "your_webhook_secret"
}
```

### Webhook Events

**Job Completed:**
```json
{
  "event": "job.completed",
  "timestamp": "2025-11-21T12:05:00Z",
  "data": {
    "job_id": "job_789",
    "project_id": "proj_123",
    "result_url": "https://cdn.craudiovizai.com/..."
  }
}
```

**Credits Low:**
```json
{
  "event": "credits.low",
  "timestamp": "2025-11-21T12:05:00Z",
  "data": {
    "current_balance": 50,
    "threshold": 100
  }
}
```

### Verify Webhook Signature
```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hmac = crypto.createHmac('sha256', secret);
  const digest = hmac.update(payload).digest('hex');
  return signature === digest;
}
```

---

## Errors

### Error Format
```json
{
  "error": {
    "code": "insufficient_credits",
    "message": "You need 10 credits but have 5",
    "details": {
      "required": 10,
      "available": 5
    }
  }
}
```

### Common Error Codes

| Code | Status | Meaning |
|------|--------|---------|
| `invalid_api_key` | 401 | API key is invalid |
| `rate_limit_exceeded` | 429 | Too many requests |
| `insufficient_credits` | 402 | Not enough credits |
| `project_not_found` | 404 | Project doesn't exist |
| `invalid_input` | 400 | Request validation failed |
| `server_error` | 500 | Internal server error |

---

## SDKs

### JavaScript/Node.js
```bash
npm install @craudiovizai/sdk
```

```javascript
const CRAudioViz = require('@craudiovizai/sdk');

const client = new CRAudioViz({
  apiKey: 'YOUR_API_KEY'
});

// Create project
const project = await client.projects.create({
  name: 'My Project'
});

// Use tool
const job = await client.tools.execute('logo_maker', {
  project_id: project.id,
  inputs: {
    text: 'My Company'
  }
});

// Wait for completion
const result = await client.jobs.wait(job.id);
console.log('Logo created:', result.url);
```

### Python
```bash
pip install craudiovizai
```

```python
from craudiovizai import CRAudioViz

client = CRAudioViz(api_key='YOUR_API_KEY')

# Create project
project = client.projects.create(
    name='My Project'
)

# Use tool
job = client.tools.execute('logo_maker', 
    project_id=project.id,
    inputs={
        'text': 'My Company'
    }
)

# Wait for completion
result = client.jobs.wait(job.id)
print(f'Logo created: {result.url}')
```

### PHP
```bash
composer require craudiovizai/sdk
```

```php
<?php
use CRAudioViz\Client;

$client = new Client([
    'api_key' => 'YOUR_API_KEY'
]);

// Create project
$project = $client->projects->create([
    'name' => 'My Project'
]);

// Use tool
$job = $client->tools->execute('logo_maker', [
    'project_id' => $project->id,
    'inputs' => [
        'text' => 'My Company'
    ]
]);

// Wait for completion
$result = $client->jobs->wait($job->id);
echo "Logo created: {$result->url}";
```

---

## Code Examples

### Create Logo with API
```python
import requests
import time

API_KEY = 'YOUR_API_KEY'
BASE_URL = 'https://api.craudiovizai.com/v1'

def create_logo(company_name):
    # Create project
    response = requests.post(
        f'{BASE_URL}/projects',
        headers={'Authorization': f'Bearer {API_KEY}'},
        json={'name': f'{company_name} Branding'}
    )
    project = response.json()
    
    # Execute logo tool
    response = requests.post(
        f'{BASE_URL}/tools/logo_maker/execute',
        headers={'Authorization': f'Bearer {API_KEY}'},
        json={
            'project_id': project['id'],
            'inputs': {
                'text': company_name,
                'style': 'modern'
            }
        }
    )
    job = response.json()
    
    # Poll for completion
    while True:
        response = requests.get(
            f'{BASE_URL}/jobs/{job["job_id"]}',
            headers={'Authorization': f'Bearer {API_KEY}'}
        )
        status = response.json()
        
        if status['status'] == 'completed':
            return status['result']['url']
        elif status['status'] == 'failed':
            raise Exception(status.get('error'))
        
        time.sleep(2)

# Use it
logo_url = create_logo('Acme Corp')
print(f'Logo ready: {logo_url}')
```

### Batch Process Images
```javascript
const CRAudioViz = require('@craudiovizai/sdk');
const client = new CRAudioViz({ apiKey: 'YOUR_API_KEY' });

async function batchResize(images, width, height) {
  const jobs = [];
  
  // Upload all images
  for (const image of images) {
    const file = await client.files.upload(image);
    const job = await client.tools.execute('image_resizer', {
      inputs: {
        file_id: file.id,
        width,
        height
      }
    });
    jobs.push(job);
  }
  
  // Wait for all to complete
  const results = await Promise.all(
    jobs.map(job => client.jobs.wait(job.id))
  );
  
  return results.map(r => r.url);
}

// Use it
const resizedUrls = await batchResize(
  ['image1.jpg', 'image2.jpg', 'image3.jpg'],
  1000,
  1000
);
```

### Monitor Credits with Webhook
```javascript
// Express.js webhook endpoint
const express = require('express');
const crypto = require('crypto');
const app = express();

app.post('/webhook', express.raw({type: 'application/json'}), (req, res) => {
  const signature = req.headers['x-craudioviz-signature'];
  const payload = req.body;
  
  // Verify signature
  const hmac = crypto.createHmac('sha256', 'YOUR_WEBHOOK_SECRET');
  const digest = hmac.update(payload).digest('hex');
  
  if (signature !== digest) {
    return res.status(401).send('Invalid signature');
  }
  
  const event = JSON.parse(payload);
  
  if (event.event === 'credits.low') {
    console.log('⚠️ Low credits warning!');
    console.log(`Current balance: ${event.data.current_balance}`);
    
    // Auto-purchase more credits
    purchaseCredits(500);
  }
  
  res.status(200).send('OK');
});

app.listen(3000);
```

---

## Best Practices

### API Key Security
- ✅ Store in environment variables
- ✅ Never commit to Git
- ✅ Rotate keys every 90 days
- ✅ Use different keys per environment
- ❌ Never expose in client-side code

### Rate Limit Handling
```javascript
async function apiRequest(url, options) {
  try {
    const response = await fetch(url, options);
    
    if (response.status === 429) {
      // Rate limited - wait and retry
      const resetTime = response.headers.get('X-RateLimit-Reset');
      const waitTime = resetTime - Date.now();
      await new Promise(resolve => setTimeout(resolve, waitTime));
      return apiRequest(url, options);
    }
    
    return response;
  } catch (error) {
    console.error('API request failed:', error);
    throw error;
  }
}
```

### Error Handling
```python
from craudiovizai import CRAudioViz, APIError, RateLimitError

client = CRAudioViz(api_key='YOUR_API_KEY')

try:
    result = client.tools.execute('logo_maker', {...})
except RateLimitError as e:
    # Handle rate limit
    time.sleep(e.retry_after)
    result = client.tools.execute('logo_maker', {...})
except APIError as e:
    # Handle other API errors
    print(f'API Error: {e.code} - {e.message}')
    raise
```

### Caching
```javascript
const cache = new Map();

async function getCachedProject(projectId) {
  if (cache.has(projectId)) {
    return cache.get(projectId);
  }
  
  const project = await client.projects.get(projectId);
  cache.set(projectId, project);
  
  // Clear cache after 5 minutes
  setTimeout(() => cache.delete(projectId), 5 * 60 * 1000);
  
  return project;
}
```

---

## Testing

### Sandbox Environment
```
https://sandbox.craudiovizai.com/v1
```

**Sandbox Features:**
- Free unlimited requests
- Doesn't consume real credits
- Test data only
- No charges to card

### Test API Keys
Get sandbox keys: Dashboard → API Keys → Sandbox

---

## Support

### Developer Resources
- 📚 Full docs: [developers.craudiovizai.com](https://developers.craudiovizai.com)
- 💬 Discord: #developers channel
- 📧 Email: developers@craudiovizai.com
- 📖 GitHub: [github.com/CR-AudioViz-AI](https://github.com/CR-AudioViz-AI)

### Community
- Stack Overflow: Tag `craudiovizai`
- Reddit: r/craudiovizai
- Twitter: @craudiovizai_dev

---

## Changelog

### v1.0.0 (November 2025)
- Initial API release
- Core endpoints: projects, tools, files
- Webhook support
- SDK for JS, Python, PHP

### Coming Soon
- GraphQL endpoint
- Real-time websocket API
- Batch operations
- Advanced filtering
- More SDK languages

---

**Questions? Contact developers@craudiovizai.com**

---

*API documentation updated monthly. Last update: November 21, 2025*
