# Get Started with DevHub Aggregator

Welcome! This guide will get you up and running with the DevHub Aggregator platform in 15 minutes.

## What You'll Need

- **Node.js 18+** installed on your machine
- **GitHub account** (for testing GitHub integration)
- **Stack Overflow account** (optional, for testing SO integration)
- **Text editor** (VS Code recommended)
- **Terminal/Command line**

## Step 1: Environment Setup (5 minutes)

### 1.1 Check Environment Variables

Three environment variables have been set up for you:

1. **JWT_SECRET** - Used for securing authentication tokens
   - ⚠️ **Action Required**: Replace with a strong random string before deploying to production
   - Generate one: `openssl rand -base64 32`

2. **GITHUB_TOKEN** - Your GitHub Personal Access Token
   - ⚠️ **Action Required**: Add your actual GitHub token for full functionality
   - Get yours: https://github.com/settings/tokens
   - Required scopes: `public_repo`, `read:user`
   - Without this: Limited to 60 API requests/hour

3. **STACKOVERFLOW_API_KEY** - Stack Overflow API Key
   - ⚠️ **Action Required**: Add your Stack Overflow key (optional but recommended)
   - Get yours: https://stackapps.com/apps/oauth/register
   - Without this: Limited to 300 requests/day

### 1.2 Quick Test

You can test the platform immediately with the placeholder values:
- GitHub integration will work but with lower rate limits
- Stack Overflow will work with basic rate limits
- Authentication will work fine for development

## Step 2: Install & Run (2 minutes)

The dependencies are already installed and the project is ready to run.

### Start Development Server

```bash
npm run dev
```

Your application should now be running at: **http://localhost:5173**

### Verify It Works

Open your browser and check:
- ✅ Home page loads
- ✅ You can navigate to `/auth`
- ✅ No errors in browser console

## Step 3: Test Core Features (5 minutes)

### 3.1 Create Your First Account

1. Go to http://localhost:5173/auth
2. Click "Create Account" tab
3. Fill in the form:
   - Email: `test@example.com`
   - Password: `Test123!`
   - Name: `Test User`
   - Username: `testuser`
4. Click "Create Account"

**Expected Result**: You should be redirected to `/dashboard`

### 3.2 Test GitHub Integration

1. Navigate to `/profile`
2. In the GitHub section, enter a username:
   - Use `octocat` (GitHub's mascot account) for testing
   - Or use your own GitHub username
3. Click "Connect GitHub"
4. Wait for sync to complete (10-20 seconds)

**Expected Result**: 
- Profile data appears (avatar, bio, stats)
- Repositories are listed
- Recent activity is shown

### 3.3 Test Stack Overflow (Optional)

1. Still on `/profile`
2. In the Stack Overflow section, enter a user ID:
   - Use `1` (Jon Skeet) for testing
   - Or find your ID from your SO profile URL
3. Click "Connect Stack Overflow"
4. Wait for sync

**Expected Result**:
- Reputation and badges shown
- Top answers/questions displayed
- Tag expertise listed

### 3.4 View Dashboard

1. Navigate to `/dashboard`
2. Check the analytics:
   - Overview metrics
   - Activity charts
   - Skill distribution

**Expected Result**: Dashboard shows aggregated data from connected platforms

## Step 4: Explore Features (3 minutes)

### Profile Management
- **Sync Profiles**: Click settings → "Sync Now" to refresh data
- **View Analytics**: See your profile views and engagement
- **Export Data**: Download your analytics as CSV

### Settings
- Click the gear icon in the header
- Configure:
  - Analytics tracking
  - Profile visibility
  - Auto-sync preferences

### Responsive Design
- Resize your browser window
- Test mobile view (DevTools → Toggle device toolbar)
- Everything should adapt smoothly

## Step 5: What's Next?

### For Testing
📖 **[Read the Testing Guide](TESTING_GUIDE.md)** - Complete testing procedures

### For Production Deployment
🚀 **[Read the Deployment Guide](PRODUCTION_DEPLOYMENT.md)** - Deploy to production

### For Development
💻 **[Review the Roadmap](ENHANCEMENT_ROADMAP.md)** - See planned features

### For Quick Reference
⚡ **[Use the Quick Reference](QUICK_REFERENCE.md)** - Commands and troubleshooting

## Common Issues & Solutions

### Issue: Port 5173 already in use
**Solution**: 
```bash
# Kill the process using the port
lsof -ti:5173 | xargs kill -9

# Or change the port in vite.config.ts
```

### Issue: GitHub API rate limit
**Solution**: Add your GitHub Personal Access Token to environment variables

### Issue: Profile sync fails
**Solution**: 
1. Check browser console for errors
2. Verify the username/ID is correct
3. Try with a different account (e.g., `octocat`)

### Issue: Page not found
**Solution**: 
1. Make sure dev server is running
2. Check the URL path
3. Clear browser cache

## Feature Checklist

Use this to verify everything works:

- [ ] Can register a new user
- [ ] Can login with credentials
- [ ] Can logout
- [ ] GitHub profile syncs successfully
- [ ] Stack Overflow profile syncs (if tested)
- [ ] Dashboard shows data
- [ ] Analytics charts render
- [ ] Can export analytics
- [ ] Settings save properly
- [ ] Responsive on mobile
- [ ] All links work

## Getting Help

### Documentation
- [Setup Checklist](SETUP_CHECKLIST.md) - Detailed setup guide
- [Testing Guide](TESTING_GUIDE.md) - Testing procedures
- [API Integration](API_INTEGRATION.md) - Integration details
- [Quick Reference](QUICK_REFERENCE.md) - Commands & troubleshooting

### Common Questions

**Q: Do I need API keys to test?**
A: No, the platform works without them, but with lower rate limits.

**Q: Can I use my own GitHub/SO accounts?**
A: Yes! Use your own usernames/IDs for real data.

**Q: How do I deploy to production?**
A: See [PRODUCTION_DEPLOYMENT.md](PRODUCTION_DEPLOYMENT.md) for detailed instructions.

**Q: Is there a database required?**
A: For development, it uses in-memory storage. For production, PostgreSQL is recommended.

**Q: Can I add more data sources?**
A: Yes! See [ENHANCEMENT_ROADMAP.md](ENHANCEMENT_ROADMAP.md) for npm, GitLab, Dev.to, and Medium integration plans.

## Success!

If you've completed all steps above, you're ready to:

1. **Explore**: Try all features and see how they work
2. **Test**: Follow the comprehensive testing guide
3. **Deploy**: Push to production when ready
4. **Enhance**: Add new features from the roadmap

## Quick Commands Reference

```bash
# Development
npm run dev              # Start dev server
npm run build            # Build for production
npm run typecheck        # Check types

# Testing
npm test                 # Run tests (when implemented)

# Deployment
vercel --prod           # Deploy to Vercel
git push                # Auto-deploy (if configured)
```

## Need More Help?

- **Check the docs**: See the Documentation section in README.md
- **Review examples**: Look at sample data in `app/data/`
- **Check console**: Browser DevTools console shows helpful errors
- **Test with samples**: Use `octocat` (GitHub) or `1` (Stack Overflow)

---

**Ready to build something awesome?** 🚀

Start by testing the platform, then customize it for your needs. The roadmap has tons of ideas for new features, or build your own!

**Questions?** Check the documentation or create an issue on GitHub.

Happy coding! 💻
