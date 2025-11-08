# Javari AI - User Guide

**Last Updated:** November 8, 2025  
**Version:** 1.0  
**For:** End Users (Customers)  
**Platform:** javariai.com

---

## WELCOME TO JAVARI AI

Javari AI is your autonomous development assistant - not just a chatbot, but a true AI partner that builds applications, fixes broken code, and continuously learns from every interaction. This guide will help you get the most out of Javari.

---

## GETTING STARTED

### Step 1: Create Your Account

**Free Tier (No Credit Card Required):**
1. Visit [javariai.com](https://javariai.com)
2. Click "Get Started Free"
3. Sign up with email or OAuth (Google, GitHub)
4. Verify your email
5. You get 10 credits to start ($0.10 worth of AI usage)

**Pro Tier ($49/month):**
- Unlimited credits
- Priority support
- Early access to new features
- Higher rate limits

**Enterprise ($499/month):**
- White-label branding
- Custom integrations
- Dedicated support
- Multi-user teams

### Step 2: Set Up Your First Project

Projects organize all your work in one place:

1. Click "New Project" in dashboard
2. Enter project name (e.g., "My Business Website")
3. Add optional description
4. Link GitHub repository (optional but recommended)
5. Click "Create Project"

**What Projects Include:**
- All conversations related to this project
- Encrypted credentials (API keys, tokens)
- Knowledge base files (documentation you upload)
- Build history and deployments
- Cost tracking specific to this project

### Step 3: Add Credentials (Secure)

Javari needs credentials to deploy your apps:

**Required Credentials:**
- GitHub Token (for code commits)
- Vercel Token (for deployments)

**Optional Credentials:**
- OpenAI API Key (if you want to use your own)
- Stripe Keys (for payment processing apps)
- Other service credentials as needed

**How to Add:**
1. Go to Project Settings → Credentials
2. Click "Add Credential"
3. Select credential type
4. Paste your API key/token
5. Mark "Inherited by sub-projects" if you want child projects to use it
6. Click "Save" (automatically encrypted with AES-256-GCM)

**Security Note:** Your credentials are encrypted and never visible to anyone, including CR AudioViz AI staff. You can test if they work without revealing the actual value.

---

## BASIC USAGE

### Starting a Conversation

**Simple Chat:**
```
You: "Hi Javari, I need help building a website"
Javari: "I'd love to help! Let me ask a few questions first:
1. What's the purpose of the website?
2. Who is the target audience?
3. What features do you need?
4. Do you have any branding (colors, logo)?
5. What's your timeline?"
```

Javari will ask clarifying questions before building anything.

### Building Your First App

**Example: Invoice Generator**

```
You: "Build me a PDF invoice generator"

Javari: "I'll create a professional invoice generator! Here's my plan:

📋 Features:
- Company details input form
- Line items with quantity, price, tax
- Automatic total calculation
- Professional PDF export
- Downloadable file
- Mobile-responsive design

💻 Tech Stack:
- Next.js 14 with TypeScript
- Tailwind CSS for styling
- react-pdf for generation
- Form validation with Zod

⏱️ Estimated Time: 10-12 minutes
💰 Cost: 75 credits ($0.75)

Ready to proceed?"

You: "Yes, go ahead"

Javari: [Builds application]
✅ Created 12 files (783 lines of code)
✅ Committed to GitHub: CR-AudioViz-AI/invoice-generator
✅ Deployed to Vercel: invoice-generator-abc123.vercel.app
✅ Total time: 11 minutes, 23 seconds
✅ Cost: 73 credits ($0.73)

🔗 Preview: https://invoice-generator-abc123.vercel.app
📁 GitHub: https://github.com/CR-AudioViz-AI/invoice-generator

Try it out and let me know if you need any changes!
```

### Making Changes

**Iterative Refinement:**
```
You: "Add a logo upload feature"

Javari: "I'll add logo upload capability. Changes:
1. New file upload component
2. Image processing and optimization
3. Logo display in invoice header
4. Optional logo positioning

⏱️ Time: 3-4 minutes
💰 Cost: 15 credits ($0.15)

Proceeding..."

[Updates application]
✅ Modified 4 files (+127 lines)
✅ Committed and deployed
✅ Total time: 3 minutes, 41 seconds

Logo upload is live! Try uploading your company logo.
```

### Getting Help

**Ask Anything:**
```
You: "How do I change the default font?"

Javari: "To change fonts in your app:

1. **Using Tailwind (recommended):**
   Edit `tailwind.config.js`:
   ```js
   fontFamily: {
     sans: ['Inter', 'sans-serif'],
     serif: ['Merriweather', 'serif']
   }
   ```

2. **Using Google Fonts:**
   Add to `app/layout.tsx`:
   ```tsx
   import { Inter } from 'next/font/google'
   const inter = Inter({ subsets: ['latin'] })
   ```

3. **Custom fonts:**
   Place font files in `public/fonts/`
   Import in your CSS

Want me to apply any of these changes?"
```

---

## ADVANCED FEATURES

### Chat Continuation

**Why It Matters:**
AI conversations have token limits (like word count). When you approach the limit, Javari automatically suggests continuing to a new chat.

**How It Works:**
```
Javari: "⚠️ This chat is 90% full (115,000 / 128,000 tokens).
Would you like to continue to a new chat?

✅ All context will be preserved
✅ Conversation summary auto-generated
✅ You won't lose any information

[Continue Chat] [Keep This Chat]"
```

**Benefits:**
- Never lose context
- Unlimited conversation length
- Easy to navigate between parent/child chats
- Complete project history maintained

### Knowledge Base (Upload Files)

**What You Can Upload:**
- PDFs (documentation, specifications)
- Word documents (DOCX)
- Text files (README, notes)
- Code files (for reference)

**How to Upload:**
```
1. Open conversation
2. Click paperclip icon
3. Select file(s)
4. Javari automatically:
   - Extracts text
   - Generates embeddings
   - Makes content searchable
   - Uses in future responses
```

**Example Use Case:**
```
[You upload product-requirements.pdf]

You: "Build a dashboard based on the requirements doc"

Javari: "I've read your requirements document. I see you need:
- User authentication
- Dashboard with 6 widgets
- Real-time data updates
- Export to CSV functionality
- Mobile responsive design

I'll build this based on your specs. Estimated time: 15 minutes."
```

Javari automatically references uploaded documents when relevant!

### Project Hierarchy

**Main Projects vs. Sub-Projects:**

**Main Project: "My Business"**
- Credentials: GITHUB_TOKEN, VERCEL_TOKEN
- These are inherited by ALL sub-projects

**Sub-Projects (inherit parent credentials):**
- "Business Website" (uses parent credentials)
- "Invoice Generator" (uses parent credentials + STRIPE_KEY)
- "Customer Portal" (uses parent credentials + SUPABASE_KEY)

**Benefits:**
- Add credentials once, use everywhere
- Override when needed
- Organized workspace
- Easy cost tracking per sub-project

### Starred Conversations

**Mark Important Chats:**
1. Open conversation
2. Click star icon ⭐
3. Find starred chats in "Starred" filter

**Use Cases:**
- Important decisions
- Best practices discovered
- Templates to reuse
- Reference conversations

---

## UNDERSTANDING COSTS

### Credit System

**What is a Credit?**
- 1 credit = $0.01 USD
- Free tier: 10 credits/month
- Pro tier: Unlimited credits (included in $49/month)

**How Credits Are Used:**
- AI tokens (input + output)
- API calls to external services
- Database operations (negligible)

**Examples:**
- Simple chat message: 0.5-2 credits ($0.005-$0.02)
- Code generation (small): 5-10 credits ($0.05-$0.10)
- Build full application: 50-100 credits ($0.50-$1.00)
- Complex multi-file app: 100-200 credits ($1.00-$2.00)

### Cost Tracking

**Real-Time Display:**
Every Javari response shows:
```
💰 Cost: 12 credits ($0.12)
📊 Tokens: 3,450 (input) + 8,230 (output)
```

**Monthly Reports:**
```
November 2025 Usage:
- Total credits used: 487 ($4.87)
- Most expensive project: Website Redesign (213 credits)
- Average per conversation: 15 credits
- Projected end-of-month: $12.50

Recommendation: Upgrade to Pro for unlimited credits
```

### Cost Optimization Tips

**1. Be Specific in Requests:**
- ❌ "Build me a website" (vague, many follow-ups)
- ✅ "Build a 5-page business website with contact form, about page, services page, blog, and homepage"

**2. Upload Documentation:**
Instead of explaining requirements in chat (uses tokens), upload a PDF document (one-time cost)

**3. Use Simple Tasks for Iteration:**
- ❌ "Rebuild the entire app with different colors"
- ✅ "Change the primary color from blue to green"

**4. Batch Related Changes:**
- ❌ Three separate conversations for three small changes
- ✅ "Make these 3 changes: 1) add logo, 2) fix footer, 3) update contact email"

---

## BEST PRACTICES

### Writing Effective Prompts

**Good Prompts Include:**
1. **Clear objective:** What you want to build/accomplish
2. **Specific details:** Features, requirements, constraints
3. **Context:** Who will use it, why it's needed
4. **Success criteria:** How you'll know it's done correctly

**Example - Bad Prompt:**
```
"Make me a website"
```
(Javari will ask 10+ clarifying questions, wasting credits)

**Example - Good Prompt:**
```
"Build a business website for my plumbing company with:
- Homepage featuring 3 services (repairs, installations, inspections)
- About page with company history and team photos
- Contact form with name, email, phone, service needed
- Service area map (zip code validator)
- Mobile-responsive design
- Professional blue/white color scheme
- Call-to-action buttons throughout
Target audience: Homeowners age 35-65 in Houston area
Timeline: Launch in 1 week"
```
(Javari can start building immediately with minimal follow-up)

### Leveraging Javari's Memory

**Project Context:**
Javari remembers everything about your project:
```
You: "What color scheme did we use for my website?"
Javari: "Your business website uses professional blue (#1E40AF) and white (#FFFFFF), with accent green (#10B981) for call-to-action buttons. We chose this based on your plumbing industry and trust-building goals."
```

**Conversation History:**
```
You: "Make the same changes to my invoice generator"
Javari: "I'll apply the logo upload feature we added to your website. Same implementation?"
```

**Learning Preferences:**
Over time, Javari learns your preferences:
- Coding style (comments, TypeScript strict mode)
- Design aesthetic (minimal, bold, professional)
- Feature priorities (mobile-first, accessibility)
- Communication style (technical detail vs. high-level)

### Collaboration Workflow

**For Teams:**

1. **Create Organization Project:**
   - Main project for company
   - Add all team credentials
   - Enable credential inheritance

2. **Invite Team Members:**
   - Each member creates account
   - Request access to organization project
   - Admin approves with permission level

3. **Work in Sub-Projects:**
   - Each person creates sub-projects
   - Inherits organization credentials
   - Independent work doesn't conflict

4. **Review Before Merge:**
   - Javari shows all changes
   - Preview deployments for testing
   - Manual approval for production

---

## TROUBLESHOOTING

### Common Issues

**Issue: "Build Failed - Deployment Error"**

**Solution:**
```
You: "My build failed"

Javari: "I see the deployment error. Let me diagnose...

Error: Missing environment variable NEXT_PUBLIC_API_KEY

This means your Vercel project needs an API key configured.

🔧 Auto-Fix Options:
1. I can add it to Vercel for you (using your credentials)
2. You can add it manually in Vercel dashboard
3. I can modify code to work without it

Which would you prefer? I recommend option 1 for fastest resolution."
```

**Issue: "Chat Not Responding"**

**Possible Causes:**
- Rate limit reached (wait 1 minute)
- Network connection issue (check internet)
- Server maintenance (check status page)

**Solution:**
1. Refresh page
2. Check [status.craudiovizai.com](https://status.craudiovizai.com)
3. Contact support if persists

**Issue: "Incorrect Feature Built"**

**Solution:**
```
You: "This isn't what I wanted"

Javari: "I apologize for the misunderstanding. Let me clarify what you need:
1. What specifically is incorrect?
2. What should it do instead?
3. Can you provide an example or reference?

Once I understand correctly, I'll rebuild it at no additional cost."
```

### Getting Support

**Self-Service:**
1. Search knowledge base ([help.craudiovizai.com](https://help.craudiovizai.com))
2. Ask Javari directly: "How do I...?"
3. Check status page for outages

**Contact Support:**
- **Email:** support@craudiovizai.com
- **Response Time:** 
  - Free: 48 hours
  - Pro: 24 hours
  - Enterprise: 4 hours
- **Chat Support:** Pro/Enterprise only

---

## SECURITY & PRIVACY

### Your Data is Safe

**Encryption:**
- All credentials encrypted with AES-256-GCM
- Cannot be viewed by anyone (including staff)
- Automatic key rotation every 90 days

**Data Storage:**
- US-based servers (Supabase in Ohio)
- GDPR compliant
- SOC 2 Type II certified
- HIPAA-ready (Enterprise tier)

**Access Control:**
- Two-factor authentication (2FA) available
- IP whitelisting (Enterprise)
- Session timeout after 30 minutes inactivity
- Complete audit log of all actions

### What We Do With Your Data

**We Use Data For:**
- ✅ Providing service to you
- ✅ Improving Javari's AI models (anonymized)
- ✅ Customer support
- ✅ Security monitoring

**We Never:**
- ❌ Sell your data to third parties
- ❌ Use your code in other customers' projects
- ❌ Train public AI models on your proprietary code
- ❌ Share credentials across users

### Deleting Your Data

**Account Deletion:**
```
Settings → Account → Delete Account

⚠️ This will permanently delete:
- All conversations
- All projects
- All credentials
- All files uploaded
- Cannot be undone

Exported data will be emailed before deletion.
```

**Data Retention:**
- Active accounts: Indefinite
- Deleted accounts: 30 days (then permanently purged)
- Backups: 90 days (then purged)

---

## TIPS & TRICKS

### Pro Tips from Power Users

**1. Template Conversations:**
Create a starred conversation with your preferred setup:
```
"Build a Next.js app with:
- TypeScript strict mode
- Tailwind CSS
- ESLint + Prettier
- Supabase authentication
- Vercel deployment
- Mobile-first design
- Dark mode support"
```
Reference this in future conversations: "Use my standard template"

**2. Batch Operations:**
Instead of one change at a time, batch related changes:
```
"For all my apps:
1. Update footer copyright to 2025
2. Add Google Analytics
3. Update contact email to hello@company.com"
```

**3. Use Inheritance:**
Add common credentials to parent project once, inherit everywhere:
- GitHub token
- Vercel token
- Google Analytics ID
- Sentry DSN
- Stripe publishable key

**4. Knowledge Base as Documentation:**
Upload your brand guidelines, code standards, and requirements as PDFs. Javari automatically follows them in all projects.

**5. Conversation Naming:**
Rename conversations to describe what was built:
- ❌ "Chat with Javari"
- ✅ "Invoice Generator with Logo Upload"

### Keyboard Shortcuts

- `Ctrl/Cmd + K`: Quick search
- `Ctrl/Cmd + Enter`: Send message
- `Ctrl/Cmd + N`: New conversation
- `Ctrl/Cmd + ,`: Open settings
- `Esc`: Close modals

---

## PRICING & PLANS

### Free Tier
**$0/month**
- 10 credits per month
- Access to all core features
- Community support
- 1 project
- Basic analytics

**Best For:** Trying Javari, hobby projects

### Pro Tier
**$49/month**
- **Unlimited credits**
- Everything in Free, plus:
- Unlimited projects
- Priority support (24hr response)
- Advanced analytics
- Custom domains
- 2FA security

**Best For:** Freelancers, small businesses, active developers

### Enterprise Tier
**$499/month**
- Everything in Pro, plus:
- White-label branding
- Custom integrations
- Dedicated support (4hr response)
- Multi-user teams
- SSO authentication
- SLA guarantee (99.9% uptime)
- HIPAA compliance

**Best For:** Agencies, large businesses, compliance-required organizations

### Add-Ons

**Extra Credits (Free Tier Only):**
- $10 = 1,000 credits
- Never expire
- Rollover month to month

**Priority Support:**
- $29/month
- 4-hour response time
- Phone support available

---

## FREQUENTLY ASKED QUESTIONS

**Q: How is Javari different from ChatGPT?**
A: ChatGPT writes code that you have to manually deploy. Javari writes code AND deploys it automatically to Vercel, commits to GitHub, and manages the entire lifecycle. Plus, Javari has infinite memory through conversation continuation and learns your preferences over time.

**Q: Can Javari edit existing code?**
A: Yes! Give Javari access to your GitHub repository, and it can read, modify, and update existing applications.

**Q: What if Javari builds something wrong?**
A: Javari will rebuild or fix it at no additional cost. We also track patterns to avoid repeating mistakes.

**Q: Is my API key safe?**
A: Absolutely. We use AES-256-GCM encryption (military-grade) and zero-knowledge architecture. We literally cannot see your credentials even if we wanted to.

**Q: Can multiple people use one account?**
A: Free and Pro tiers are single-user. Enterprise tier supports teams with role-based access control.

**Q: What happens if I run out of credits?**
A: (Free tier) Javari stops working until next month or you purchase credits. (Pro/Enterprise) You have unlimited credits - never run out.

**Q: Can I export my code?**
A: Yes! All code is in YOUR GitHub repository. You own it 100%. You can download, modify, or take it anywhere.

**Q: Does Javari work offline?**
A: No, Javari requires internet connection to function.

**Q: What programming languages does Javari support?**
A: Primarily JavaScript/TypeScript (React, Next.js, Node.js). Python support coming Q1 2026.

**Q: Can Javari build mobile apps?**
A: Currently web applications only. Mobile apps (React Native) coming Q2 2026.

---

## GETTING MORE HELP

### Resources

**Documentation:**
- User Guide (this document)
- API Reference: [docs.craudiovizai.com/api](https://docs.craudiovizai.com/api)
- Video Tutorials: [youtube.com/@craudiovizai](https://youtube.com/@craudiovizai)

**Community:**
- Discord: [discord.gg/craudiovizai](https://discord.gg/craudiovizai)
- Reddit: [r/javari](https://reddit.com/r/javari)
- Twitter: [@craudiovizai](https://twitter.com/craudiovizai)

**Support:**
- Email: support@craudiovizai.com
- Knowledge Base: [help.craudiovizai.com](https://help.craudiovizai.com)
- Status Page: [status.craudiovizai.com](https://status.craudiovizai.com)

---

## WHAT'S NEXT?

### Upcoming Features (2026 Roadmap)

**Q1 2026:**
- Voice input/output (talk to Javari)
- Mobile app (iOS + Android)
- Python support
- Database design tools

**Q2 2026:**
- React Native mobile apps
- API testing tools
- Performance optimization recommendations
- Multi-language support (Spanish, French)

**Q3 2026:**
- Visual design editor (drag-and-drop)
- E-commerce tools (Shopify integration)
- Marketing automation
- Advanced analytics dashboard

**Q4 2026:**
- Video editing tools
- Social media scheduler
- Email marketing platform
- CMS (content management system)

### Request Features

**Have an idea?**
1. Go to Settings → Feature Requests
2. Describe your idea
3. Vote on others' ideas
4. Track implementation status

Top-voted features get priority development!

---

## SUCCESS STORIES

### Case Study: Local Plumbing Business

**Challenge:** Needed website but couldn't afford $5K+ developer

**Solution:** Used Javari to build complete website in 2 hours

**Results:**
- Cost: $3.50 (Pro tier: $49/month)
- Time: 2 hours vs. 2-4 weeks
- Features: 5 pages, contact form, service area map, mobile-responsive
- ROI: Gained 23 new customers in first month ($15K revenue)

**Quote:** *"I can't believe I built a professional website in 2 hours. Javari asked me questions like a real web developer would, then just... built it. I've already recommended it to 5 other business owners."* - Mike T., Houston TX

---

## CONCLUSION

Javari AI is your partner in building digital products. Whether you're a business owner with no technical skills or a developer looking to 10x your productivity, Javari adapts to your needs and grows with you.

**Key Takeaways:**
- Start small, build confidence
- Upload documentation for better results
- Use inheritance to avoid repetition
- Batch related changes together
- Trust Javari's memory - it never forgets

**Ready to Start Building?**

[Get Started Free →](https://javariai.com/signup)

---

**Need Help?**  
Email: support@craudiovizai.com  
Live Chat: Available for Pro/Enterprise  
Response Time: 24-48 hours

---

*Last Updated: November 8, 2025 | Version 1.0 | © 2025 CR AudioViz AI, LLC*
