# DEPLOYMENT GUIDE

## STANDARD DEPLOYMENT PROCESS

### 1. Local Development
```bash
npm run dev
# Test thoroughly
```

### 2. Build Test
```bash
npm run build
# Must succeed with zero errors
```

### 3. Push to GitHub
```bash
git add .
git commit -m "feat: descriptive message"
git push origin main
```

### 4. Verify Preview
- Vercel auto-creates preview deployment
- Test preview URL thoroughly
- Check all functionality

### 5. Promote to Production (Manual)
- Go to Vercel dashboard
- Click deployment
- Click "Promote to Production"
- Verify production URL

## TROUBLESHOOTING

**TypeScript Errors:**
- Run: `npx tsc --noEmit`
- Fix all type errors before pushing
- Use strict mode always

**Build Failures:**
- Check Vercel logs
- Fix errors locally
- Test build succeeds
- Push again

**503 Errors:**
- Check Vercel deployment status
- Verify environment variables
- Check for runtime errors
- Promote working preview
