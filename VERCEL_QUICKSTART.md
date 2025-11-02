# Vercel Deployment - Quick Start

## The Problem You're Facing

You're getting "files too large" error when pushing to GitHub because you're trying to push `node_modules/` folder, which contains hundreds of megabytes of dependencies.

**NEVER push `node_modules/` to Git!**

## Solution: Deploy Without Git

Since you have the code locally (downloaded from Dazl), you can deploy directly to Vercel using the CLI - no GitHub needed!

---

## Option 1: Direct Deploy via Vercel CLI (Fastest)

### Step 1: Install Vercel CLI

```bash
npm install -g vercel
```

### Step 2: Navigate to Your Project

```bash
cd /path/to/your/downloaded/aethex-link
```

### Step 3: Make Sure node_modules Exists Locally

```bash
npm install
```

### Step 4: Deploy to Vercel

```bash
# Login to Vercel
vercel login

# Deploy to production
vercel --prod
```

The CLI will:
1. Ask you to login (opens browser)
2. Upload your code (excluding `node_modules` automatically)
3. Build on Vercel's servers
4. Give you a production URL

### Step 5: Add Environment Variables

After deployment, add your secrets in Vercel Dashboard:

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Select your project
3. Settings → Environment Variables
4. Add these:

```
DATABASE_URL=your_database_url
JWT_SECRET=your_jwt_secret
GITHUB_TOKEN=your_github_token
STACKOVERFLOW_API_KEY=your_stackoverflow_key
FREENAME_API_KEY=your_freename_key
FREENAME_RESELLER_ID=your_reseller_id
```

### Step 6: Configure Custom Domains

In Vercel Dashboard → Domains:

1. Add `aethex.me`
2. Add `*.aethex.me`
3. Vercel will give you DNS records

### Step 7: Update DNS at Spaceship

Add these records to `aethex.me` in Spaceship:

```
Type: A
Name: @
Value: 76.76.21.21
TTL: 3600

Type: CNAME
Name: *
Value: cname.vercel-dns.com
TTL: 3600
```

**Done!** Your app is live at `aethex.me` and all subdomains work (`username.aethex.me`)

---

## Option 2: Fix Git and Use GitHub (If You Want CI/CD)

### Step 1: Remove node_modules from Git

```bash
cd /path/to/your/aethex-link

# Remove node_modules if you accidentally added it
git rm -rf --cached node_modules

# Make sure .gitignore has node_modules
echo "node_modules/" >> .gitignore

# Commit
git add .
git commit -m "Remove node_modules, add to .gitignore"
```

### Step 2: Create GitHub Repository

1. Go to [github.com](https://github.com)
2. Click "New Repository"
3. Name it `aethex-link`
4. **Don't initialize** (you already have code)
5. Click "Create Repository"

### Step 3: Push to GitHub

```bash
git remote add origin https://github.com/AeThex-Corporation/aethex-link.git
git branch -M main
git push -u origin main
```

### Step 4: Connect to Vercel

1. Go to [vercel.com](https://vercel.com)
2. Click "Import Project"
3. Select your GitHub repo
4. Click "Deploy"

---

## What Files Should You Push to Git?

✅ **DO push:**
- All `.ts`, `.tsx`, `.css` files
- `package.json`
- `package-lock.json`
- `README.md`, documentation
- `public/` folder
- Configuration files (`.gitignore`, `vercel.json`, etc.)

❌ **DON'T push:**
- `node_modules/` (too large, auto-installed from package.json)
- `build/` or `dist/` (generated on Vercel)
- `.env` files (secrets, add to Vercel dashboard instead)
- `.DS_Store` (Mac files)
- `.react-router/` (build cache)

---

## Troubleshooting

### "Error: File size exceeds 100 MB"

You're trying to push `node_modules/`. Remove it:

```bash
git rm -rf --cached node_modules
git commit -m "Remove node_modules"
```

### "Error: Repository size exceeds 1 GB"

Same issue. Clean your Git history:

```bash
# Start fresh
rm -rf .git
git init
git add .
git commit -m "Initial commit"
```

### "Vercel can't find package.json"

You're in the wrong directory. Make sure you're in the project root where `package.json` exists:

```bash
ls -la  # Should see package.json
```

---

## After Deployment

Once deployed, your app will be live at:
- `your-project.vercel.app` (Vercel's URL)
- `aethex.me` (after DNS configured)
- `*.aethex.me` (all user subdomains)

Test it:
1. Visit `aethex.me` - Should show your home page
2. Create a test account
3. Visit `yourUsername.aethex.me` - Should show your portfolio
4. Upgrade to premium `.aethex` domain

---

## Need Help?

If deployment fails, check:
1. Vercel build logs (in dashboard)
2. Environment variables are set correctly
3. Database is accessible from Vercel
4. DNS has propagated (use https://dnschecker.org)

You're deploying a **React Router v7** app with:
- Subdomain routing (already configured in `vercel.json`)
- Multi-platform integrations
- Blockchain domain sales
- User profiles and authentication

Everything is ready to go! Just use `vercel --prod` and you're live!
