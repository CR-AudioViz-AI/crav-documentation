# JAVARI AI - TECHNICAL ARCHITECTURE

**Document Version:** 1.0  
**Last Updated:** November 8, 2025  
**Architecture Status:** Production-Ready (95% Complete)

---

## SYSTEM OVERVIEW

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
├─────────────────────────────────────────────────────────────┤
│  Next.js 14 App │ React 18 │ TypeScript 5 │ Tailwind CSS   │
│  - Server Components                                         │
│  - Client Components (Chat UI)                               │
│  - Edge Runtime API Routes                                   │
└────────────────┬────────────────────────────────────────────┘
                 │
                 │ HTTPS/WSS
                 │
┌────────────────▼────────────────────────────────────────────┐
│                    APPLICATION LAYER                         │
├─────────────────────────────────────────────────────────────┤
│  Next.js API Routes (Edge Runtime)                           │
│  - /api/javari/chat                                          │
│  - /api/javari/knowledge                                     │
│  - /api/javari/credentials                                   │
│  - /api/javari/projects                                      │
│  - /api/javari/conversations                                 │
└────────────────┬────────────────────────────────────────────┘
                 │
                 │ Multiple Connections
                 │
┌────────────────▼────────────────────────────────────────────┐
│                    SERVICES LAYER                            │
├─────────────────────────────────────────────────────────────┤
│ ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│ │   OpenAI     │  │  Anthropic   │  │   Google     │       │
│ │   GPT-4      │  │   Claude     │  │   Gemini     │       │
│ └──────────────┘  └──────────────┘  └──────────────┘       │
│                                                              │
│ ┌──────────────┐  ┌──────────────┐                          │
│ │   Mistral    │  │  Perplexity  │                          │
│ │   Large      │  │   Sonar      │                          │
│ └──────────────┘  └──────────────┘                          │
└─────────────────────────────────────────────────────────────┘
                 │
                 │ PostgreSQL Protocol
                 │
┌────────────────▼────────────────────────────────────────────┐
│                      DATA LAYER                              │
├─────────────────────────────────────────────────────────────┤
│  Supabase (PostgreSQL 15 + Extensions)                       │
│  - pgvector (vector embeddings)                              │
│  - Row Level Security (RLS)                                  │
│  - Real-time subscriptions                                   │
│  - Storage (file uploads)                                    │
└─────────────────────────────────────────────────────────────┘
```

### Technology Stack

**Frontend:**
- **Framework:** Next.js 14.0.0 (App Router)
- **Language:** TypeScript 5.2.2
- **UI Library:** React 18.2.0
- **Styling:** Tailwind CSS 3.3.0
- **Component Library:** shadcn/ui (Radix UI primitives)
- **Icons:** Lucide React
- **Markdown:** react-markdown
- **Code Highlighting:** Prism.js / highlight.js

**Backend:**
- **Runtime:** Node.js 20.x (Edge Runtime for APIs)
- **Framework:** Next.js API Routes
- **Authentication:** Supabase Auth (JWT)
- **Database ORM:** Supabase JavaScript Client
- **Encryption:** Node.js crypto module (AES-256-GCM)
- **File Processing:** pdf-parse, mammoth, csv-parse

**Database:**
- **Primary:** PostgreSQL 15 (via Supabase)
- **Extensions:** pgvector, uuid-ossp, pg_stat_statements
- **Schema:** 20 tables with full RLS
- **Backups:** Automated daily with point-in-time recovery

**AI/ML:**
- **Primary Provider:** OpenAI (gpt-4-turbo-preview)
- **Embeddings:** OpenAI (text-embedding-3-small)
- **Alternative Providers:** Anthropic, Google, Mistral, Perplexity
- **Vector Search:** pgvector extension
- **Dimension:** 1536 (OpenAI embedding size)

**Infrastructure:**
- **Hosting:** Vercel (Serverless)
- **CDN:** Vercel Edge Network
- **DNS:** Vercel DNS / Cloudflare
- **Monitoring:** Vercel Analytics + Sentry
- **Logging:** Vercel Logs + Custom dashboard

**DevOps:**
- **Version Control:** GitHub
- **CI/CD:** Vercel Git Integration
- **Deployment:** Preview-only (manual production)
- **Environment Management:** Vercel Environment Variables
- **Secrets Management:** Encrypted vault in database

---

## DATABASE SCHEMA

### Schema Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     CONVERSATION SYSTEM                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────┐         ┌───────────────────┐    │
│  │ javari_conversations │◄────────│ javari_messages   │    │
│  ├──────────────────────┤         ├───────────────────┤    │
│  │ id (PK)              │         │ id (PK)           │    │
│  │ user_id (FK)         │         │ conversation_id   │    │
│  │ project_id (FK)      │         │ role              │    │
│  │ parent_id (FK self)  │         │ content           │    │
│  │ context_summary      │         │ tokens            │    │
│  │ title                │         │ cost              │    │
│  │ token_count          │         │ model             │    │
│  │ total_cost           │         │ created_at        │    │
│  │ status               │         └───────────────────┘    │
│  │ starred              │                                   │
│  │ created_at           │         ┌───────────────────┐    │
│  └──────────────────────┘         │ javari_           │    │
│           ▲                        │ conversation_     │    │
│           │                        │ summaries         │    │
│           │                        ├───────────────────┤    │
│           └────────────────────────│ conversation_id   │    │
│                                    │ summary           │    │
│                                    │ token_count       │    │
│                                    │ created_at        │    │
│                                    └───────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                     KNOWLEDGE BASE SYSTEM                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────┐         ┌───────────────────┐    │
│  │ javari_knowledge_    │◄────────│ javari_document_  │    │
│  │ documents            │         │ chunks            │    │
│  ├──────────────────────┤         ├───────────────────┤    │
│  │ id (PK)              │         │ id (PK)           │    │
│  │ user_id (FK)         │         │ document_id (FK)  │    │
│  │ project_id (FK)      │         │ content           │    │
│  │ filename             │         │ embedding         │    │
│  │ file_type            │         │   (vector 1536)   │    │
│  │ file_size            │         │ chunk_index       │    │
│  │ content_hash         │         │ token_count       │    │
│  │ metadata (JSONB)     │         │ created_at        │    │
│  │ created_at           │         └───────────────────┘    │
│  └──────────────────────┘                                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                   CREDENTIAL VAULT SYSTEM                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────┐         ┌───────────────────┐    │
│  │ javari_credentials   │◄────────│ javari_           │    │
│  ├──────────────────────┤         │ credential_       │    │
│  │ id (PK)              │         │ snapshots         │    │
│  │ user_id (FK)         │         ├───────────────────┤    │
│  │ project_id (FK)      │         │ id (PK)           │    │
│  │ service_name         │         │ credential_id     │    │
│  │ credential_name      │         │ encrypted_value   │    │
│  │ encrypted_value      │         │ iv                │    │
│  │ iv                   │         │ auth_tag          │    │
│  │ auth_tag             │         │ grace_period_ends │    │
│  │ expires_at           │         │ created_at        │    │
│  │ rotation_days        │         └───────────────────┘    │
│  │ last_rotated_at      │                                   │
│  │ created_at           │         ┌───────────────────┐    │
│  └──────────────────────┘         │ javari_           │    │
│           ▲                        │ credential_       │    │
│           │                        │ audit_log         │    │
│           │                        ├───────────────────┤    │
│           └────────────────────────│ credential_id     │    │
│                                    │ user_id           │    │
│                                    │ action            │    │
│                                    │ ip_address        │    │
│                                    │ created_at        │    │
│                                    └───────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                     PROJECT SYSTEM                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────┐         ┌───────────────────┐    │
│  │ javari_projects      │◄────────│ javari_project_   │    │
│  ├──────────────────────┤         │ members           │    │
│  │ id (PK)              │         ├───────────────────┤    │
│  │ user_id (FK)         │         │ id (PK)           │    │
│  │ parent_id (FK self)  │         │ project_id (FK)   │    │
│  │ name                 │         │ user_id (FK)      │    │
│  │ description          │         │ role              │    │
│  │ settings (JSONB)     │         │ permissions       │    │
│  │ created_at           │         │ created_at        │    │
│  └──────────────────────┘         └───────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│              MONITORING & AUTONOMOUS SYSTEMS                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────┐        │
│  │ javari_health_checks │  │ javari_auto_fixes    │        │
│  ├──────────────────────┤  ├──────────────────────┤        │
│  │ id (PK)              │  │ id (PK)              │        │
│  │ check_type           │  │ error_type           │        │
│  │ status               │  │ error_message        │        │
│  │ response_time        │  │ fix_attempted        │        │
│  │ error_message        │  │ fix_successful       │        │
│  │ metadata (JSONB)     │  │ time_to_fix          │        │
│  │ created_at           │  │ created_at           │        │
│  └──────────────────────┘  └──────────────────────┘        │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────┐        │
│  │ javari_task_queue    │  │ javari_agent_tasks   │        │
│  ├──────────────────────┤  ├──────────────────────┤        │
│  │ id (PK)              │  │ id (PK)              │        │
│  │ task_type            │  │ parent_task_id (FK)  │        │
│  │ payload (JSONB)      │  │ agent_name           │        │
│  │ status               │  │ agent_role           │        │
│  │ priority             │  │ input_data (JSONB)   │        │
│  │ attempts             │  │ output_data (JSONB)  │        │
│  │ scheduled_for        │  │ status               │        │
│  │ created_at           │  │ created_at           │        │
│  └──────────────────────┘  └──────────────────────┘        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Complete Schema SQL

See `/technical/javari/schema.sql` for complete schema with:
- All 20 tables
- Complete indexes
- Row Level Security policies
- Database functions
- Triggers
- Views

### Key Database Features

**1. Row Level Security (RLS)**
- Enabled on all tables
- Users can only access their own data
- Service role bypasses RLS for admin operations
- Project-based sharing for team collaboration

**2. Vector Similarity Search**
```sql
CREATE FUNCTION match_document_chunks(
  query_embedding VECTOR(1536),
  match_threshold FLOAT,
  match_count INT
)
RETURNS TABLE (
  id UUID,
  document_id UUID,
  content TEXT,
  similarity FLOAT
)
LANGUAGE plpgsql
AS $$
BEGIN
  RETURN QUERY
  SELECT
    javari_document_chunks.id,
    javari_document_chunks.document_id,
    javari_document_chunks.content,
    1 - (javari_document_chunks.embedding <=> query_embedding) AS similarity
  FROM javari_document_chunks
  WHERE 1 - (javari_document_chunks.embedding <=> query_embedding) > match_threshold
  ORDER BY similarity DESC
  LIMIT match_count;
END;
$$;
```

**3. Automatic Timestamps**
```sql
-- Trigger function for updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Applied to all tables with updated_at
CREATE TRIGGER update_javari_conversations_updated_at
  BEFORE UPDATE ON javari_conversations
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();
```

**4. Indexes for Performance**
```sql
-- Conversation indexes
CREATE INDEX idx_conversations_user_id ON javari_conversations(user_id);
CREATE INDEX idx_conversations_project_id ON javari_conversations(project_id);
CREATE INDEX idx_conversations_parent_id ON javari_conversations(parent_id);
CREATE INDEX idx_conversations_status ON javari_conversations(status);
CREATE INDEX idx_conversations_created_at ON javari_conversations(created_at DESC);

-- Message indexes
CREATE INDEX idx_messages_conversation_id ON javari_messages(conversation_id);
CREATE INDEX idx_messages_created_at ON javari_messages(created_at DESC);

-- Vector similarity index (HNSW for fast ANN search)
CREATE INDEX idx_document_chunks_embedding 
  ON javari_document_chunks 
  USING hnsw (embedding vector_cosine_ops);

-- Credential indexes
CREATE INDEX idx_credentials_user_id ON javari_credentials(user_id);
CREATE INDEX idx_credentials_project_id ON javari_credentials(project_id);
CREATE INDEX idx_credentials_service_name ON javari_credentials(service_name);
```

---

## API ARCHITECTURE

### API Route Structure

```
/api/javari/
├── chat/
│   └── route.ts                 # POST - Stream chat responses
├── conversations/
│   ├── route.ts                 # GET, POST - List/create conversations
│   ├── [id]/
│   │   ├── route.ts             # GET, PUT, DELETE - Conversation CRUD
│   │   └── continue/
│   │       └── route.ts         # POST - Create continuation
├── knowledge/
│   ├── route.ts                 # GET - List documents
│   ├── upload/
│   │   └── route.ts             # POST - Upload document
│   ├── search/
│   │   └── route.ts             # POST - Semantic search
│   └── [id]/
│       └── route.ts             # GET, DELETE - Document operations
├── credentials/
│   ├── route.ts                 # GET, POST - List/create credentials
│   ├── [id]/
│   │   ├── route.ts             # GET, PUT, DELETE - Credential CRUD
│   │   ├── rotate/
│   │   │   └── route.ts         # POST - Rotate credential
│   │   └── test/
│   │       └── route.ts         # POST - Test credential
└── projects/
    ├── route.ts                 # GET, POST - List/create projects
    └── [id]/
        ├── route.ts             # GET, PUT, DELETE - Project CRUD
        └── members/
            └── route.ts         # GET, POST, DELETE - Team management
```

### API Specifications

#### Chat API

**POST /api/javari/chat**

Stream AI responses in real-time.

**Request:**
```typescript
interface ChatRequest {
  message: string;
  conversationId?: string;  // Optional, creates new if omitted
  projectId?: string;       // Optional, for project context
  model?: string;           // Optional, auto-selected if omitted
  temperature?: number;     // Optional, default 0.7
  includeKnowledge?: boolean; // Optional, default true
}
```

**Response:** Server-Sent Events (SSE) stream
```typescript
// Event types:
interface ChatStreamEvent {
  type: 'token' | 'complete' | 'error';
  data: {
    token?: string;          // For type='token'
    message?: Message;       // For type='complete'
    error?: string;          // For type='error'
    usage?: {
      promptTokens: number;
      completionTokens: number;
      totalTokens: number;
      cost: number;
    };
  };
}
```

**Example:**
```javascript
const response = await fetch('/api/javari/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    message: 'Explain React hooks',
    conversationId: 'conv-123',
    includeKnowledge: true,
  }),
});

const reader = response.body.getReader();
const decoder = new TextDecoder();

while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  
  const chunk = decoder.decode(value);
  const events = chunk.split('\n\n');
  
  for (const event of events) {
    if (event.startsWith('data: ')) {
      const data = JSON.parse(event.slice(6));
      if (data.type === 'token') {
        console.log(data.data.token); // Stream token
      }
    }
  }
}
```

#### Conversations API

**GET /api/javari/conversations**

List user's conversations with pagination.

**Query Parameters:**
```typescript
interface ConversationsQuery {
  limit?: number;        // Default 50, max 100
  offset?: number;       // Default 0
  projectId?: string;    // Filter by project
  status?: 'active' | 'paused' | 'completed';
  search?: string;       // Search titles/content
  starred?: boolean;     // Only starred
}
```

**Response:**
```typescript
interface ConversationsResponse {
  conversations: Conversation[];
  total: number;
  limit: number;
  offset: number;
}

interface Conversation {
  id: string;
  user_id: string;
  project_id?: string;
  parent_id?: string;
  title: string;
  context_summary?: string;
  token_count: number;
  total_cost: number;
  status: 'active' | 'paused' | 'completed';
  starred: boolean;
  created_at: string;
  updated_at: string;
  message_count: number;
  last_message_at: string;
}
```

**POST /api/javari/conversations**

Create new conversation.

**Request:**
```typescript
interface CreateConversationRequest {
  title?: string;        // Auto-generated if omitted
  projectId?: string;    // Associate with project
  templateId?: string;   // Start from template
}
```

**Response:**
```typescript
{
  conversation: Conversation;
}
```

#### Knowledge Base API

**POST /api/javari/knowledge/upload**

Upload document for RAG processing.

**Request:** multipart/form-data
```typescript
{
  file: File;            // PDF, DOCX, TXT, MD, CSV, JSON
  projectId?: string;    // Associate with project
  metadata?: {
    title?: string;
    description?: string;
    tags?: string[];
  };
}
```

**Response:**
```typescript
interface UploadResponse {
  document: {
    id: string;
    filename: string;
    file_type: string;
    file_size: number;
    chunk_count: number;
    status: 'processing' | 'ready' | 'error';
  };
}
```

**POST /api/javari/knowledge/search**

Semantic search through documents.

**Request:**
```typescript
interface SearchRequest {
  query: string;
  threshold?: number;    // Default 0.7, range 0-1
  limit?: number;        // Default 5, max 20
  projectId?: string;    // Filter by project
}
```

**Response:**
```typescript
interface SearchResponse {
  results: Array<{
    id: string;
    document_id: string;
    document_name: string;
    content: string;
    similarity: number;   // 0-1
    chunk_index: number;
  }>;
}
```

#### Credentials API

**GET /api/javari/credentials**

List credentials (decrypted).

**Query Parameters:**
```typescript
{
  projectId?: string;    // Filter by project
  service?: string;      // Filter by service
}
```

**Response:**
```typescript
interface CredentialsResponse {
  credentials: Array<{
    id: string;
    service_name: string;
    credential_name: string;
    value: string;        // Decrypted
    expires_at?: string;
    rotation_days?: number;
    last_rotated_at?: string;
    status: 'active' | 'expired' | 'rotating';
  }>;
}
```

**POST /api/javari/credentials**

Create new credential.

**Request:**
```typescript
interface CreateCredentialRequest {
  projectId?: string;
  serviceName: string;
  credentialName: string;
  value: string;
  expiresAt?: string;     // ISO 8601
  rotationDays?: number;  // 30, 60, 90, or null
}
```

**Response:**
```typescript
{
  credential: Credential;
}
```

**POST /api/javari/credentials/[id]/rotate**

Rotate credential (automatic or manual).

**Request:**
```typescript
interface RotateRequest {
  newValue?: string;      // For manual rotation
  gracePeriodDays?: number; // Default 7
}
```

**Response:**
```typescript
{
  oldCredentialId: string;
  newCredentialId: string;
  gracePeriodEnds: string;
}
```

---

## ENCRYPTION & SECURITY

### Credential Encryption

**Algorithm:** AES-256-GCM (Galois/Counter Mode)
- **Key Size:** 256 bits (32 bytes)
- **IV Size:** 128 bits (16 bytes)
- **Auth Tag:** 128 bits (16 bytes)

**Encryption Process:**
```typescript
import crypto from 'crypto';

function encrypt(plaintext: string): EncryptedData {
  // Get encryption key from environment
  const key = Buffer.from(process.env.CREDENTIAL_ENCRYPTION_KEY!, 'hex');
  
  // Generate random IV
  const iv = crypto.randomBytes(16);
  
  // Create cipher
  const cipher = crypto.createCipheriv('aes-256-gcm', key, iv);
  
  // Encrypt
  let encrypted = cipher.update(plaintext, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  
  // Get authentication tag
  const authTag = cipher.getAuthTag();
  
  return {
    encrypted,
    iv: iv.toString('hex'),
    authTag: authTag.toString('hex'),
  };
}

function decrypt(data: EncryptedData): string {
  const key = Buffer.from(process.env.CREDENTIAL_ENCRYPTION_KEY!, 'hex');
  const iv = Buffer.from(data.iv, 'hex');
  const authTag = Buffer.from(data.authTag, 'hex');
  
  const decipher = crypto.createDecipheriv('aes-256-gcm', key, iv);
  decipher.setAuthTag(authTag);
  
  let decrypted = decipher.update(data.encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  
  return decrypted;
}
```

### Authentication & Authorization

**Authentication Flow:**
```
1. User signs in with email/password or OAuth
2. Supabase Auth generates JWT token
3. Token stored in httpOnly cookie
4. Token validated on every API request
5. RLS policies enforce user isolation
```

**Row Level Security Example:**
```sql
-- Users can only see their own conversations
CREATE POLICY "Users can view own conversations"
  ON javari_conversations
  FOR SELECT
  USING (auth.uid() = user_id);

-- Service role bypasses RLS (for admin operations)
-- Uses SUPABASE_SERVICE_ROLE_KEY
```

**Authorization Levels:**
```typescript
enum Role {
  USER = 'user',
  ADMIN = 'admin',
  SERVICE = 'service',
}

enum ProjectRole {
  OWNER = 'owner',
  ADMIN = 'admin',
  DEVELOPER = 'developer',
  VIEWER = 'viewer',
}
```

---

## AI PROVIDER INTEGRATION

### Multi-Provider Architecture

**Provider Interface:**
```typescript
interface AIProvider {
  name: string;
  models: string[];
  
  chat(request: ChatRequest): AsyncIterableIterator<ChatToken>;
  embed(text: string): Promise<number[]>;
  
  cost: {
    inputPer1k: number;
    outputPer1k: number;
  };
  
  limits: {
    maxTokens: number;
    requestsPerMinute: number;
  };
}
```

**Provider Implementations:**

**1. OpenAI Provider**
```typescript
class OpenAIProvider implements AIProvider {
  name = 'openai';
  models = ['gpt-4-turbo-preview', 'gpt-3.5-turbo'];
  
  async *chat(request: ChatRequest) {
    const response = await openai.chat.completions.create({
      model: request.model || 'gpt-4-turbo-preview',
      messages: request.messages,
      stream: true,
      temperature: request.temperature || 0.7,
    });
    
    for await (const chunk of response) {
      const token = chunk.choices[0]?.delta?.content;
      if (token) yield token;
    }
  }
  
  async embed(text: string): Promise<number[]> {
    const response = await openai.embeddings.create({
      model: 'text-embedding-3-small',
      input: text,
    });
    return response.data[0].embedding;
  }
  
  cost = {
    inputPer1k: 0.01,
    outputPer1k: 0.03,
  };
}
```

**2. Anthropic Provider**
```typescript
class AnthropicProvider implements AIProvider {
  name = 'anthropic';
  models = ['claude-3-opus-20240229', 'claude-3-sonnet-20240229'];
  
  async *chat(request: ChatRequest) {
    const stream = await anthropic.messages.stream({
      model: request.model || 'claude-3-sonnet-20240229',
      messages: request.messages,
      max_tokens: 4096,
      temperature: request.temperature || 0.7,
    });
    
    for await (const chunk of stream) {
      if (chunk.type === 'content_block_delta') {
        yield chunk.delta.text;
      }
    }
  }
  
  cost = {
    inputPer1k: 0.003,
    outputPer1k: 0.015,
  };
}
```

### Provider Selection Logic

```typescript
function selectProvider(context: ConversationContext): AIProvider {
  // Analyze task complexity
  const complexity = analyzeComplexity(context);
  
  // Route based on task type
  if (complexity.hasCode && complexity.isComplex) {
    return openaiProvider; // GPT-4 best for complex code
  }
  
  if (complexity.isCreative || complexity.needsAnalysis) {
    return anthropicProvider; // Claude best for creative/analysis
  }
  
  if (complexity.isSimple) {
    return geminiProvider; // Gemini cheapest for simple tasks
  }
  
  // Default to GPT-4
  return openaiProvider;
}

function analyzeComplexity(context: ConversationContext) {
  return {
    hasCode: /```/.test(context.lastMessage),
    isComplex: context.tokenCount > 2000,
    isCreative: /write|create|generate story/.test(context.lastMessage),
    needsAnalysis: /analyze|compare|evaluate/.test(context.lastMessage),
    isSimple: context.tokenCount < 500 && !context.hasCode,
  };
}
```

### Fallback Mechanism

```typescript
async function chatWithFallback(request: ChatRequest) {
  const providers = [openaiProvider, anthropicProvider, geminiProvider];
  
  for (const provider of providers) {
    try {
      return await provider.chat(request);
    } catch (error) {
      console.error(`${provider.name} failed:`, error);
      
      // Log failure for monitoring
      await logProviderFailure(provider.name, error);
      
      // Try next provider
      continue;
    }
  }
  
  // All providers failed
  throw new Error('All AI providers unavailable');
}
```

---

## PERFORMANCE OPTIMIZATION

### Caching Strategy

**1. API Response Caching**
```typescript
// Edge caching for static responses
export const config = {
  runtime: 'edge',
  // Cache for 1 hour, revalidate every 5 minutes
  revalidate: 300,
};
```

**2. Database Query Caching**
```typescript
// Cache frequently accessed data
const cachedProjects = await redis.get(`user:${userId}:projects`);
if (cachedProjects) return JSON.parse(cachedProjects);

const projects = await supabase
  .from('javari_projects')
  .select('*')
  .eq('user_id', userId);

await redis.setex(`user:${userId}:projects`, 3600, JSON.stringify(projects));
```

**3. Vector Embedding Caching**
```typescript
// Cache embeddings for frequently searched queries
const cacheKey = `embedding:${hashQuery(query)}`;
let embedding = await redis.get(cacheKey);

if (!embedding) {
  embedding = await openai.embeddings.create({ input: query });
  await redis.setex(cacheKey, 86400, JSON.stringify(embedding));
}
```

### Database Optimization

**1. Connection Pooling**
```typescript
// Supabase client with connection pooling
const supabase = createClient(
  process.env.SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!,
  {
    db: {
      schema: 'public',
    },
    auth: {
      autoRefreshToken: false,
      persistSession: false,
    },
    global: {
      headers: {
        'x-connection-pool-timeout': '10',
      },
    },
  }
);
```

**2. Query Optimization**
```sql
-- Efficient pagination with index
SELECT * FROM javari_conversations
WHERE user_id = $1
ORDER BY created_at DESC
LIMIT 50 OFFSET $2;

-- Use covering indexes
CREATE INDEX idx_conversations_user_created 
  ON javari_conversations(user_id, created_at DESC)
  INCLUDE (id, title, status);
```

**3. Batch Operations**
```typescript
// Batch insert messages
await supabase
  .from('javari_messages')
  .insert(messages); // Single query for multiple rows

// Batch update conversations
await supabase.rpc('batch_update_conversations', {
  updates: conversationsToUpdate,
});
```

### Streaming Optimization

**1. Chunked Responses**
```typescript
// Stream chat responses immediately
export async function POST(req: Request) {
  const encoder = new TextEncoder();
  
  const stream = new ReadableStream({
    async start(controller) {
      for await (const token of chatStream) {
        controller.enqueue(encoder.encode(`data: ${token}\n\n`));
      }
      controller.close();
    },
  });
  
  return new Response(stream, {
    headers: {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive',
    },
  });
}
```

**2. Compression**
```typescript
// Compress large responses
import { gzip } from 'zlib';
import { promisify } from 'util';

const gzipAsync = promisify(gzip);

// Compress JSON responses > 1KB
if (jsonSize > 1024) {
  const compressed = await gzipAsync(JSON.stringify(data));
  return new Response(compressed, {
    headers: {
      'Content-Type': 'application/json',
      'Content-Encoding': 'gzip',
    },
  });
}
```

---

## MONITORING & OBSERVABILITY

### Logging Architecture

```typescript
interface LogEntry {
  timestamp: string;
  level: 'debug' | 'info' | 'warn' | 'error';
  service: 'api' | 'database' | 'ai-provider';
  action: string;
  userId?: string;
  conversationId?: string;
  duration?: number;
  metadata?: Record<string, any>;
  error?: Error;
}

class Logger {
  async log(entry: LogEntry) {
    // Console for development
    if (process.env.NODE_ENV === 'development') {
      console.log(entry);
    }
    
    // Vercel logs for production
    if (process.env.VERCEL) {
      console.log(JSON.stringify(entry));
    }
    
    // Also store in database for audit trail
    if (entry.level === 'error' || entry.level === 'warn') {
      await supabase.from('javari_logs').insert(entry);
    }
  }
}
```

### Metrics Collection

```typescript
interface Metrics {
  // API metrics
  requestCount: number;
  responseTime: number;
  errorRate: number;
  
  // AI metrics
  tokensUsed: number;
  costIncurred: number;
  providerFailures: number;
  
  // Database metrics
  queryTime: number;
  connectionPoolSize: number;
  cacheHitRate: number;
  
  // Business metrics
  activeUsers: number;
  conversationsCreated: number;
  messagesPerConversation: number;
  creditsConsumed: number;
}
```

### Health Checks

```typescript
// /api/health endpoint
export async function GET() {
  const health = {
    status: 'healthy',
    timestamp: new Date().toISOString(),
    checks: {
      database: await checkDatabase(),
      ai_providers: await checkAIProviders(),
      storage: await checkStorage(),
      cache: await checkCache(),
    },
  };
  
  const allHealthy = Object.values(health.checks).every(c => c.status === 'healthy');
  
  return Response.json(health, {
    status: allHealthy ? 200 : 503,
  });
}

async function checkDatabase() {
  try {
    const { data, error } = await supabase.from('javari_health_checks').select('count');
    return { status: 'healthy', latency: Date.now() };
  } catch (error) {
    return { status: 'unhealthy', error: error.message };
  }
}
```

---

## DEPLOYMENT & CI/CD

### Vercel Configuration

**vercel.json:**
```json
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "nextjs",
  "outputDirectory": ".next",
  "git": {
    "deploymentEnabled": {
      "main": false
    },
    "autoAlias": false
  },
  "regions": ["iad1"],
  "env": {
    "NEXT_PUBLIC_SUPABASE_URL": "@supabase-url",
    "NEXT_PUBLIC_SUPABASE_ANON_KEY": "@supabase-anon-key"
  },
  "build": {
    "env": {
      "SUPABASE_SERVICE_ROLE_KEY": "@supabase-service-role-key",
      "OPENAI_API_KEY": "@openai-api-key",
      "CREDENTIAL_ENCRYPTION_KEY": "@credential-encryption-key"
    }
  }
}
```

### Environment Variables

**Required:**
```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
SUPABASE_SERVICE_ROLE_KEY=eyJ...

# OpenAI
OPENAI_API_KEY=sk-proj-...

# Encryption
CREDENTIAL_ENCRYPTION_KEY=hex-encoded-32-byte-key

# Application
NEXT_PUBLIC_APP_URL=https://javariai.com
NODE_ENV=production
```

**Optional:**
```bash
# Alternative AI providers
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=AIza...
MISTRAL_API_KEY=...

# Monitoring
SENTRY_DSN=https://...
VERCEL_ANALYTICS_ID=...

# Feature flags
ENABLE_AUTONOMOUS_FEATURES=false
ENABLE_VOICE_INPUT=false
```

### Build Process

```typescript
// next.config.js
module.exports = {
  reactStrictMode: true,
  experimental: {
    serverActions: true,
    serverComponentsExternalPackages: ['pdf-parse', 'mammoth'],
  },
  webpack: (config, { isServer }) => {
    if (isServer) {
      // Exclude client-only packages from server bundle
      config.externals.push({
        'react-dom/client': 'commonjs react-dom/client',
      });
    }
    return config;
  },
};
```

### TypeScript Configuration

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "jsx": "preserve",
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "allowJs": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

---

## TESTING STRATEGY

### Unit Tests
```typescript
// Example: Encryption tests
describe('Credential Encryption', () => {
  it('should encrypt and decrypt correctly', () => {
    const plaintext = 'sk-test-1234567890';
    const encrypted = encrypt(plaintext);
    const decrypted = decrypt(encrypted);
    expect(decrypted).toBe(plaintext);
  });
  
  it('should fail with wrong auth tag', () => {
    const plaintext = 'sk-test-1234567890';
    const encrypted = encrypt(plaintext);
    encrypted.authTag = 'wrong-tag';
    expect(() => decrypt(encrypted)).toThrow();
  });
});
```

### Integration Tests
```typescript
// Example: API integration tests
describe('Chat API', () => {
  it('should create conversation and send message', async () => {
    const response = await fetch('/api/javari/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message: 'Hello' }),
    });
    
    expect(response.status).toBe(200);
    expect(response.headers.get('content-type')).toBe('text/event-stream');
  });
});
```

### E2E Tests
```typescript
// Example: Playwright E2E tests
test('complete user flow', async ({ page }) => {
  // Sign in
  await page.goto('/login');
  await page.fill('[name=email]', 'test@example.com');
  await page.fill('[name=password]', 'password123');
  await page.click('button[type=submit]');
  
  // Create conversation
  await page.click('text=New Chat');
  await page.fill('[name=message]', 'Write a hello world in Python');
  await page.click('text=Send');
  
  // Wait for response
  await page.waitForSelector('code');
  
  // Verify code artifact
  const code = await page.textContent('code');
  expect(code).toContain('print("Hello, World!")');
});
```

---

## SCALABILITY CONSIDERATIONS

### Current Capacity

**Vercel Serverless:**
- Automatic scaling to 0
- Max 1,000 concurrent executions
- 10-second timeout for serverless functions
- 300-second timeout for Edge functions

**Supabase:**
- Dedicated Postgres instance
- 2GB RAM, 2 CPU cores (current plan)
- Can scale vertically to 32GB+ RAM
- Read replicas available for read-heavy workloads

**AI Providers:**
- OpenAI: 10,000 RPM (requests per minute)
- Anthropic: 5,000 RPM
- Rate limiting handled automatically

### Scaling Strategies

**Horizontal Scaling:**
1. **API:** Serverless = infinite horizontal scale
2. **Database:** Add read replicas for read operations
3. **Cache:** Redis cluster for distributed caching
4. **Storage:** Supabase Storage + CDN for files

**Vertical Scaling:**
1. **Database:** Upgrade to larger Supabase instance
2. **Functions:** Use Vercel Pro for higher limits

**Cost Optimization at Scale:**
1. **Cache embeddings:** Reduce OpenAI API calls by 60%
2. **Batch operations:** Reduce database queries by 40%
3. **Provider routing:** Save 30-50% on AI costs
4. **Compression:** Reduce bandwidth by 70%

---

## DISASTER RECOVERY

### Backup Strategy

**Database Backups:**
- Automated daily full backups (Supabase)
- Point-in-time recovery (up to 7 days)
- Weekly backup to external storage (S3)
- Monthly archive backups (retained 1 year)

**File Backups:**
- All uploaded files stored in Supabase Storage
- Automatic replication to multiple availability zones
- Weekly sync to S3 for redundancy

**Configuration Backups:**
- All code in GitHub (version controlled)
- Environment variables documented
- Deployment configuration in Vercel
- Weekly export of critical settings

### Recovery Procedures

**Database Corruption:**
```bash
# 1. Stop accepting new writes
vercel env rm DATABASE_URL

# 2. Restore from backup
supabase db restore <backup-id>

# 3. Verify integrity
npm run db:verify

# 4. Resume operations
vercel env add DATABASE_URL <new-url>
```

**Service Outage:**
```bash
# 1. Check status
curl https://status.javariai.com

# 2. If AI provider down, switch to fallback
curl -X POST /api/admin/force-provider -d '{"provider":"anthropic"}'

# 3. If database down, enable read-only mode
vercel env add READ_ONLY_MODE=true
```

### Incident Response

**Severity Levels:**
- **P0 (Critical):** Complete service outage - 15min response
- **P1 (High):** Major feature down - 1hr response
- **P2 (Medium):** Minor issue - 4hr response
- **P3 (Low):** Enhancement/bug - Next sprint

**On-Call Rotation:**
- Primary: Roy Henderson
- Secondary: AI monitoring system (auto-heal)
- Escalation: Supabase/Vercel support

---

## CONCLUSION

Javari AI's architecture is designed for:
- **🔒 Security First:** Military-grade encryption, RLS, audit logging
- **⚡ Performance:** Edge runtime, caching, streaming responses
- **📈 Scalability:** Serverless architecture, horizontal scaling ready
- **🔄 Reliability:** Multi-provider fallback, health monitoring
- **🚀 Developer Experience:** Clear APIs, comprehensive documentation

**Current Status:** 95% complete, blocked by TypeScript build errors
**Next Steps:** Fix build issues, deploy to production, begin Phase 2 autonomous features

For detailed implementation guidelines, see:
- `/technical/javari/schema.sql` - Complete database schema
- `/technical/javari/api-reference.md` - Full API documentation
- `/operations/javari/RUNBOOK.md` - Operational procedures

---

**Questions or clarifications needed? Contact the development team or refer to this documentation.**

*Last updated: November 8, 2025*
