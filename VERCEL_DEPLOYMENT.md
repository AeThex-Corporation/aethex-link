# Vercel Deployment Guide for Aethex.me Domain

## Quick Deploy to Vercel

### Step 1: Install Vercel CLI (Optional but recommended)
```bash
npm install -g vercel
```

### Step 2: Deploy via Vercel CLI
```bash
# Login to Vercel
vercel login

# Deploy (from your project root)
vercel

# Or deploy to production immediately
vercel --prod
```

### Step 3: Deploy via Vercel Dashboard (Alternative)

1. Go to [vercel.com](https://vercel.com)
2. Sign in with GitHub/GitLab/Bitbucket
3. Click "Add New Project"
4. Import your Git repository
5. Vercel will auto-detect React Router
6. Click "Deploy"

## Configure Environment Variables on Vercel

After deployment, add your secrets:

1. Go to your project on Vercel Dashboard
2. Click "Settings" → "Environment Variables"
3. Add each variable:

```env
# Required - Database & Auth
DATABASE_URL=your_database_url
JWT_SECRET=your_jwt_secret
SESSION_SECRET=your_session_secret

# Freename Domain API
FREENAME_API_URL=https://api.freename.io/v1
FREENAME_API_KEY=your_freename_api_key
FREENAME_RESELLER_ID=your_reseller_id

# Platform APIs (Optional - for integrations)
GITHUB_TOKEN=your_github_token
GITLAB_TOKEN=your_gitlab_token
DEVTO_API_KEY=your_devto_key
MEDIUM_API_KEY=your_medium_key
STACKOVERFLOW_KEY=your_stackoverflow_key
NPM_TOKEN=your_npm_token
PYPI_USERNAME=your_pypi_username
PYPI_TOKEN=your_pypi_token
HUGGINGFACE_TOKEN=your_huggingface_token
KAGGLE_USERNAME=your_kaggle_username
KAGGLE_KEY=your_kaggle_key
ROBLOX_API_KEY=your_roblox_key
UNITY_API_KEY=your_unity_key
```

4. Click "Save"
5. Redeploy your project

## Get Your Vercel IP Address

Once deployed, Vercel provides you with:

### 1. **Vercel Deployment URL**
- Format: `your-project-name.vercel.app`
- Example: `aethex-link.vercel.app`

### 2. **Get IP Address**

#### Option A: Use `nslookup`
```bash
nslookup your-project-name.vercel.app
```

#### Option B: Use `dig`
```bash
dig your-project-name.vercel.app
```

#### Option C: Use `ping`
```bash
ping your-project-name.vercel.app
```

**Note:** Vercel uses Anycast IPs that may change. Better approach below.

### 3. **Recommended: Use CNAME (Not A Record)**

Instead of using an IP address, Vercel recommends using a CNAME record:

**For aethex.me domain:**
1. Go to your domain registrar (Freename/wherever you registered aethex.me)
2. Add DNS records:

```
Type: CNAME
Name: @
Value: your-project-name.vercel.app
```

```
Type: CNAME
Name: www
Value: your-project-name.vercel.app
```

**For subdomains (*.aethex.me):**
```
Type: CNAME
Name: *
Value: your-project-name.vercel.app
```

## Add Custom Domain to Vercel

### Step 1: Add Domain in Vercel Dashboard
1. Go to your project settings
2. Click "Domains"
3. Add domain: `aethex.me`
4. Add wildcard: `*.aethex.me`

### Step 2: Vercel will provide DNS records

Vercel will show you exactly what DNS records to add:
- For root domain (aethex.me)
- For wildcard subdomains (*.aethex.me)

### Step 3: Update DNS at Freename

Go to Freename dashboard and add the DNS records Vercel provides.

**Typical records will be:**
```
A     @      76.76.21.21
CNAME www    cname.vercel-dns.com
CNAME *      cname.vercel-dns.com
```

## Subdomain Routing Setup

Your app already has subdomain routing configured in `vercel.json`:

```json
{
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

This will automatically route:
- `aethex.me` → Home page
- `username.aethex.me` → User's free subdomain
- `dashboard.aethex.me` → Dashboard (if configured)

## Verify Deployment

After DNS propagation (5 minutes - 48 hours):

1. **Check root domain:**
```bash
curl https://aethex.me
```

2. **Check subdomain:**
```bash
curl https://testuser.aethex.me
```

3. **Test in browser:**
- https://aethex.me
- https://yourname.aethex.me

## SSL/TLS Certificates

Vercel automatically provisions SSL certificates for:
- Your main domain (aethex.me)
- All subdomains (*.aethex.me)

No configuration needed - it's automatic!

## Production Checklist

Before going live:

- [ ] All environment variables added to Vercel
- [ ] Custom domain added in Vercel dashboard
- [ ] DNS records updated at domain registrar
- [ ] SSL certificate provisioned (automatic)
- [ ] Subdomain routing tested
- [ ] Database connected and initialized
- [ ] API integrations working
- [ ] Payment system configured (if applicable)

## Monitoring & Analytics

Once deployed, monitor your app:

1. **Vercel Analytics:**
   - Enable in project settings
   - Track page views, performance

2. **Vercel Logs:**
   - Real-time function logs
   - Error tracking

3. **Custom Analytics:**
   - Your app already has analytics at `/api/analytics/dashboard`

## Troubleshooting

### Domain not working?
- Wait 24-48 hours for DNS propagation
- Clear your browser cache
- Use incognito/private browsing
- Check DNS with: `dig aethex.me`

### Subdomains not routing?
- Verify wildcard CNAME record exists
- Check `vercel.json` configuration
- Ensure `X-Forwarded-Host` header is set

### Environment variables not working?
- Redeploy after adding variables
- Check spelling/naming exactly matches code
- Verify variables are in Production environment

## Next Steps After Deployment

1. **Set up database:**
   - Use Vercel Postgres or external DB
   - Run migrations/initialization

2. **Configure Freename API:**
   - Add API keys to environment variables
   - Test domain registration flow

3. **Enable features:**
   - Test OAuth connections
   - Verify platform integrations
   - Test payment flow

4. **Marketing:**
   - Share your deployed site!
   - Test user registration
   - Invite beta users

## Support

- Vercel Docs: https://vercel.com/docs
- Vercel Support: https://vercel.com/support
- React Router on Vercel: https://reactrouter.com/deploying/vercel

---

**Quick Command Summary:**
```bash
# Deploy to Vercel
vercel --prod

# Get deployment URL
vercel ls

# View logs
vercel logs

# Add domain
vercel domains add aethex.me
```

Your .aethex domain system is ready to go live! 🚀
