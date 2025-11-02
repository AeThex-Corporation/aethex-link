# Quick Start Guide

Get DevHub running in 5 minutes.

## Prerequisites

- Node.js 18+ installed
- GitHub account (optional, for real data)

## Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

Open http://localhost:5173

## First Steps

### 1. Create an Account

1. Click "Sign In" in the header
2. Go to "Sign Up" tab
3. Enter:
   - Full Name: `Your Name`
   - Email: `you@example.com`
   - Password: `yourpassword`
4. Click "Sign Up"

You'll be automatically logged in and redirected to the dashboard.

### 2. Sync Your Profile

**Without API Keys (Public Data Only):**

1. Click "Settings" (gear icon in header)
2. Go to "Connections" tab
3. Click "Sync Accounts"
4. Enter your GitHub username (e.g., `torvalds`)
5. Click "Sync Profile"

The platform will fetch your public GitHub data.

**With API Keys (Recommended):**

For better rate limits and more data:

1. Get a GitHub Personal Access Token:
   - Go to https://github.com/settings/tokens
   - Click "Generate new token (classic)"
   - Select scopes: `public_repo`, `read:user`
   - Copy the token

2. In DevHub:
   - Click "Settings" → "Connections" → "Sync Accounts"
   - Enter your GitHub username
   - Paste your GitHub token
   - (Optional) Add your Stack Overflow user ID from your profile URL
   - Click "Sync Profile"

### 3. Explore Your Dashboard

After syncing, you'll see:
- **Overview Tab**: Top skills, recent activity, achievements
- **Projects Tab**: Your repositories with stats
- **Analytics Tab**: Profile views, skill growth, source breakdown
- **Network Tab**: Connection recommendations
- **Share Tab**: Embeddable widget and sharing options

## Testing Without Your Own Data

You can explore the platform using the demo profile:

1. Go to `/profile` (without logging in)
2. View the pre-populated demo data
3. See how verified skills are calculated
4. Check out the activity timeline

## Environment Setup (Optional)

For production or advanced features:

```bash
# Create .env file
cat > .env << EOL
JWT_SECRET=$(openssl rand -base64 32)
GITHUB_TOKEN=your_github_token
STACKOVERFLOW_API_KEY=your_stackoverflow_key
EOL
```

## Common Issues

**"Port already in use"**
```bash
# Kill the process on port 5173
lsof -ti:5173 | xargs kill
```

**Build errors**
```bash
# Clear cache and reinstall
rm -rf node_modules build
npm install
```

**Type errors**
```bash
# Run type check
npm run typecheck
```

## What to Try

1. ✅ Register and login
2. ✅ Sync your GitHub profile
3. ✅ View your calculated skills
4. ✅ Check your analytics
5. ✅ Export your data as CSV
6. ✅ Share your profile link

## Next Steps

- Read [README.md](./README.md) for full documentation
- Check [DEPLOYMENT.md](./DEPLOYMENT.md) for production setup
- See [API_INTEGRATION.md](./API_INTEGRATION.md) to add more platforms
- Review [CONTRIBUTING.md](./CONTRIBUTING.md) to contribute

## Need Help?

- Check existing issues on GitHub
- Read the documentation
- Open a new issue with details

---

**Enjoy building your verified developer profile!** 🚀
