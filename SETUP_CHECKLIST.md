# Setup Checklist

Follow this checklist to get your DevHub Aggregator up and running.

## 1. Environment Setup ✓

### Required Environment Variables

The platform needs three environment variables to function properly. These are already set up in your `.env` file:

- [x] **JWT_SECRET** - For securing authentication tokens
  - Currently set to placeholder value
  - **ACTION REQUIRED**: Change to a secure random string for production
  - Generate one: `openssl rand -base64 32`

- [x] **GITHUB_TOKEN** - GitHub Personal Access Token
  - Currently set to placeholder value
  - **ACTION REQUIRED**: Add your actual GitHub token
  - Get token: https://github.com/settings/tokens
  - Required scopes: `public_repo`, `read:user`
  - Without this: Rate limited to 60 requests/hour

- [x] **STACKOVERFLOW_API_KEY** - Stack Overflow API Key
  - Currently set to placeholder value
  - **ACTION REQUIRED**: Add your actual Stack Overflow key
  - Get key: https://stackapps.com/apps/oauth/register
  - Without this: Rate limited to 300 requests/day
  - Optional but highly recommended

### How to Update Environment Variables

You have two options:

#### Option A: Update through Dazl
1. The environment variables are stored securely
2. Update them when you're ready to deploy or test with real APIs

#### Option B: Manual Update (if deploying yourself)
1. The values are stored in your `.env` file
2. Replace the placeholder values with actual keys
3. Never commit the `.env` file to version control

## 2. Installation ✓

- [x] Dependencies installed (`npm install`)
- [x] Project builds successfully (`npm run build`)
- [x] Dev server runs (`npm run dev`)

## 3. Testing Checklist

### Authentication
- [ ] Register a new user account
- [ ] Login with created account
- [ ] Logout functionality works
- [ ] Protected routes redirect to auth

### GitHub Integration
- [ ] Can enter GitHub username
- [ ] Profile syncs successfully
- [ ] Displays repositories
- [ ] Shows activity feed
- [ ] Links work correctly

### Stack Overflow Integration
- [ ] Can enter Stack Overflow user ID
- [ ] Profile syncs successfully
- [ ] Shows reputation and badges
- [ ] Displays questions/answers
- [ ] Links work correctly

### Dashboard
- [ ] Analytics display correctly
- [ ] Charts render properly
- [ ] Metrics are accurate
- [ ] Export functionality works

### Profile Page
- [ ] Combined profile displays
- [ ] All connections shown
- [ ] Skills aggregated correctly
- [ ] Can trigger manual sync

### Responsive Design
- [ ] Works on mobile (375px)
- [ ] Works on tablet (768px)
- [ ] Works on desktop (1280px+)
- [ ] No horizontal scrolling

## 4. Pre-Deployment Checklist

### Security
- [ ] Strong JWT_SECRET generated
- [ ] Environment variables secured
- [ ] No secrets in code
- [ ] CORS configured properly
- [ ] Rate limiting in place

### Database (for Production)
- [ ] PostgreSQL database provisioned
- [ ] DATABASE_URL configured
- [ ] Migrations run
- [ ] Backup strategy in place

### Performance
- [ ] Build optimization done
- [ ] Images optimized
- [ ] Code splitting enabled
- [ ] Caching configured

### Monitoring
- [ ] Error tracking setup (optional)
- [ ] Uptime monitoring (optional)
- [ ] Analytics configured (optional)

## 5. Deployment Options

Choose your deployment platform:

### Option 1: Vercel (Easiest)
- [ ] Create Vercel account
- [ ] Connect GitHub repository
- [ ] Add environment variables in Vercel dashboard
- [ ] Deploy with `vercel --prod`

### Option 2: Railway
- [ ] Create Railway account
- [ ] Add PostgreSQL database
- [ ] Configure environment variables
- [ ] Auto-deploys on git push

### Option 3: Self-Hosted VPS
- [ ] Provision VPS (2GB RAM minimum)
- [ ] Install Node.js, PostgreSQL, Nginx
- [ ] Configure reverse proxy
- [ ] Setup SSL certificate
- [ ] Configure PM2 for process management

See **PRODUCTION_DEPLOYMENT.md** for detailed deployment instructions.

## 6. Post-Deployment

- [ ] Test all features in production
- [ ] Verify environment variables work
- [ ] Check database connectivity
- [ ] Monitor error logs
- [ ] Set up automated backups

## 7. Enhancement Planning

Review **ENHANCEMENT_ROADMAP.md** for:
- [ ] Additional data sources (npm, GitLab, Dev.to)
- [ ] Recruiter search functionality
- [ ] Payment integration (Stripe)
- [ ] Advanced analytics
- [ ] Admin dashboard

## Quick Commands

```bash
# Development
npm run dev          # Start dev server
npm run build        # Build for production
npm run typecheck    # Check TypeScript errors

# Testing (after implementing tests)
npm run test         # Run unit tests
npm run test:e2e     # Run E2E tests

# Deployment
vercel --prod        # Deploy to Vercel
git push             # Auto-deploy (Railway/Vercel)

# Database (production)
npm run migrate      # Run migrations
npm run seed         # Seed initial data
```

## Getting Help

1. **Documentation**: Check the `/docs` folder
   - QUICKSTART.md - Quick start guide
   - ENV_SETUP.md - Environment setup
   - API_INTEGRATION.md - API integration details
   - TESTING_GUIDE.md - Complete testing guide
   - PRODUCTION_DEPLOYMENT.md - Deployment guide
   - ENHANCEMENT_ROADMAP.md - Feature roadmap

2. **Common Issues**:
   - **Port already in use**: Change PORT in `.env`
   - **Database connection error**: Check DATABASE_URL
   - **API rate limits**: Add GitHub token
   - **Build errors**: Run `npm install` again

3. **Support Resources**:
   - GitHub Issues: Report bugs
   - Discussions: Ask questions
   - Stack Overflow: Tag with `devhub`

## Success Criteria

Your setup is complete when:
- ✅ Application runs without errors
- ✅ Can register and login
- ✅ Can sync at least one platform (GitHub or Stack Overflow)
- ✅ Dashboard displays data
- ✅ All pages load correctly
- ✅ Responsive on all devices

## Next Steps

After completing this checklist:

1. **Test Thoroughly**: Use TESTING_GUIDE.md
2. **Deploy**: Follow PRODUCTION_DEPLOYMENT.md
3. **Plan Features**: Review ENHANCEMENT_ROADMAP.md
4. **Monitor**: Set up monitoring and analytics
5. **Iterate**: Gather feedback and improve

---

**Need immediate help?**
- Check existing documentation
- Review error messages in console
- Test with sample data first
- Verify environment variables are set correctly
