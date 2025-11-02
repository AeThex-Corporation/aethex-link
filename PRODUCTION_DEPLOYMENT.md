# Production Deployment Guide - Vercel

## Prerequisites

- Vercel account (free tier works)
- GitHub account (to connect repository)
- aethex.me domain (owned via Spaceship)
- This codebase ready to deploy

## Step 1: Prepare for Deployment

### 1.1 Environment Variables

Your app needs these environment variables in Vercel:

```
JWT_SECRET=<secret name='JWT_SECRET' displayName='JWT Secret'>*****</secret>
GITHUB_TOKEN=<secret name='GITHUB_TOKEN' displayName='GitHub Token'>*****</secret>
STACKOVERFLOW_API_KEY=<secret name='STACKOVERFLOW_API_KEY' displayName='Stack Overflow API Key'>*****</secret>
FREENAME_API_URL=https://api.freename.io/v1
FREENAME_API_KEY=your_freename_api_key_here
FREENAME_RESELLER_ID=your_reseller_id_here
```

### 1.2 Database Configuration

This app uses SQLite for development. For production, you should:

**Option A: Continue with SQLite** (Simple, but limited)
- Vercel supports SQLite in serverless functions
- Data persists between deployments
- Good for MVP/testing

**Option B: Upgrade to PostgreSQL** (Recommended for production)
- Use Vercel Postgres or Neon
- Better for concurrent users
- Scalable

## Step 2: Deploy to Vercel

### Via Vercel CLI (Recommended)

```bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Deploy
vercel

# Follow prompts:
# - Set up and deploy? Yes
# - Which scope? (your account)
# - Link to existing project? No
# - What's your project's name? aethex-platform
# - In which directory is your code? ./
# - Want to override settings? No

# This deploys to preview URL
# When ready for production:
vercel --prod
```

### Via GitHub Integration (Easier)

1. **Push code to GitHub**
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/aethex-platform.git
git push -u origin main
```

2. **Connect to Vercel**
   - Go to https://vercel.com
   - Click "Add New Project"
   - Import your GitHub repository
   - Vercel auto-detects React Router
   - Click "Deploy"

3. **Configure Environment Variables**
   - In Vercel dashboard → Project Settings → Environment Variables
   - Add all variables from Step 1.1
   - Make sure to add them for all environments (Production, Preview, Development)

## Step 3: Configure Custom Domain (aethex.me)

### 3.1 Add Domain in Vercel

1. Go to your Vercel project
2. Settings → Domains
3. Add domain: `aethex.me`
4. Add domain: `*.aethex.me` (wildcard for all subdomains)

Vercel will give you DNS records to configure.

### 3.2 Configure DNS in Spaceship

Log into Spaceship where you own aethex.me:

**For Root Domain (aethex.me):**
```
Type: A
Name: @
Value: 76.76.21.21
TTL: 3600
```

**For Wildcard Subdomains (*.aethex.me):**
```
Type: CNAME
Name: *
Value: cname.vercel-dns.com
TTL: 3600
```

**Add TXT Record (for verification):**
```
Type: TXT
Name: _vercel
Value: [Vercel will provide this]
TTL: 3600
```

> **Note:** Vercel will show you the exact values in the dashboard. The IP above (76.76.21.21) is Vercel's Anycast IP.

### 3.3 Wait for DNS Propagation

- DNS changes can take 1-48 hours
- Check status: https://dnschecker.org
- Enter: `aethex.me` and `test.aethex.me`

## Step 4: Configure Subdomain Routing

Your app already has subdomain routing built in (`subdomain.$username.tsx`), but you need to configure Vercel to handle it correctly.

Create `vercel.json` in your project root (if not exists):

```json
{
  "buildCommand": "npm run build",
  "framework": "react-router",
  "rewrites": [
    {
      "source": "/:path*",
      "destination": "/:path*"
    }
  ],
  "headers": [
    {
      "source": "/:path*",
      "headers": [
        {
          "key": "X-Forwarded-Host",
          "value": "$host"
        }
      ]
    }
  ]
}
```

## Step 5: SSL/HTTPS Configuration

Vercel automatically provides SSL certificates for:
- `aethex.me`
- `*.aethex.me` (all subdomains)

No additional configuration needed! 🎉

## Step 6: Test the Deployment

### 6.1 Test Root Domain
```
Visit: https://aethex.me
Expected: Home page loads
```

### 6.2 Test Subdomain Routing
```
Visit: https://test.aethex.me
Expected: Portfolio page for user "test" OR 404 if user doesn't exist
```

### 6.3 Create Test User
1. Visit https://aethex.me/auth
2. Register with username "johndoe"
3. Visit https://johndoe.aethex.me
4. Should see johndoe's portfolio!

## Step 7: Post-Deployment Configuration

### 7.1 Update Environment Variables

Make sure all API keys are set in Vercel dashboard.

### 7.2 Initialize Database

First deployment needs to initialize the database:

```bash
# If using Vercel Postgres:
# Vercel will run migrations automatically

# If using SQLite:
# Database initializes on first request
# Visit /auth to trigger initialization
```

### 7.3 Monitor Deployment

Vercel provides:
- Real-time logs
- Analytics
- Error tracking

Access via Vercel dashboard → Your Project → Deployments

## Troubleshooting

### Subdomains not working?

**Check DNS:**
```bash
# Check if wildcard CNAME is set
nslookup random.aethex.me
# Should return Vercel IP
```

**Check Vercel domain config:**
- Ensure `*.aethex.me` is added in Vercel domains
- Verify SSL certificate is issued

### Environment variables not working?

1. Vercel dashboard → Settings → Environment Variables
2. Make sure variables are set for "Production"
3. Redeploy after adding variables

### Database errors?

Check logs in Vercel dashboard:
```
Deployments → [Latest] → Logs
```

## Production Checklist

- [ ] Code pushed to GitHub
- [ ] Vercel project created and deployed
- [ ] Environment variables configured
- [ ] Domain `aethex.me` added to Vercel
- [ ] Wildcard domain `*.aethex.me` added to Vercel
- [ ] DNS A record for `@` → `76.76.21.21`
- [ ] DNS CNAME record for `*` → `cname.vercel-dns.com`
- [ ] DNS TXT record for `_vercel` → (verification value)
- [ ] SSL certificates issued (automatic)
- [ ] Test user registration works
- [ ] Test subdomain routing works
- [ ] Database initialized
- [ ] Analytics configured
- [ ] Error monitoring enabled

## Next Steps

After successful deployment:

1. **Marketing:** Launch announcement
2. **Monitoring:** Set up Sentry for error tracking
3. **Analytics:** Connect Google Analytics
4. **Payments:** Integrate Stripe for premium upgrades
5. **Email:** Set up SendGrid for notifications

## Support

If you encounter issues:
- Vercel Docs: https://vercel.com/docs
- React Router v7 Docs: https://reactrouter.com
- This project: Check README.md

## Cost Estimate

**Vercel Free Tier:**
- 100GB bandwidth/month
- Unlimited deployments
- Automatic SSL
- **FREE** for MVP/testing

**When you outgrow free tier:**
- Pro: $20/month
- Unlimited bandwidth
- Better performance
