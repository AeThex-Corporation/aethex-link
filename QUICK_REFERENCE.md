# Quick Reference Guide

Fast access to common tasks and commands.

## Environment Variables

```bash
# Required for production
JWT_SECRET=<generate-with-openssl-rand-base64-32>
GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxx
STACKOVERFLOW_API_KEY=xxxxxxxxxxxxxx

# Optional
DATABASE_URL=postgresql://user:pass@host:5432/db
NODE_ENV=production
PORT=3000
```

### Get API Keys

**GitHub Token**: https://github.com/settings/tokens
- Scopes: `public_repo`, `read:user`

**Stack Overflow Key**: https://stackapps.com/apps/oauth/register
- No special scopes needed

## Common Commands

```bash
# Development
npm run dev                    # Start dev server (http://localhost:5173)
npm run build                  # Build for production
npm run typecheck             # Type check

# Testing
npm test                       # Run tests (when implemented)
npm run test:e2e              # E2E tests (when implemented)

# Deployment
vercel --prod                 # Deploy to Vercel
git push                      # Auto-deploy (if configured)

# Database
npm run migrate               # Run migrations
npm run seed                  # Seed data
```

## Project Structure

```
app/
├── routes/              # Page routes
│   ├── home.tsx        # Landing page
│   ├── auth.tsx        # Login/Register
│   ├── dashboard.tsx   # Analytics dashboard
│   └── profile.tsx     # Profile management
├── components/         # React components
│   ├── ui/            # UI library
│   └── *.tsx          # Feature components
├── lib/               # Server utilities
│   ├── auth.server.ts        # Authentication
│   ├── db.server.ts          # Database
│   ├── github.server.ts      # GitHub API
│   ├── stackoverflow.server.ts
│   └── aggregation.server.ts # Data aggregation
├── hooks/             # React hooks
├── data/              # Mock/fixture data
└── styles/            # CSS files
    ├── theme.css      # Theme variables
    └── global.css     # Global styles
```

## API Endpoints

### Authentication
```
POST /api/auth/register    # Register user
POST /api/auth/login       # Login user
GET  /api/auth/me          # Get current user
```

### Profile
```
GET  /api/profile/:userId          # Get profile
POST /api/profile/sync             # Sync all platforms
GET  /api/connections              # Get connections
```

### Analytics
```
GET /api/analytics/dashboard       # Dashboard data
GET /api/analytics/export          # Export analytics
```

## Test Data

### Sample GitHub Usernames
- `octocat` - GitHub's mascot account
- `torvalds` - Linus Torvalds
- `gaearon` - Dan Abramov (React)
- `tj` - TJ Holowaychuk

### Sample Stack Overflow IDs
- `1` - Jon Skeet (legendary contributor)
- `22656` - Gordon Linoff
- `157882` - BalusC

## Deployment Platforms

### Vercel (Recommended)
```bash
npm i -g vercel
vercel login
vercel --prod
```
**Cost**: Free tier available, Pro $20/mo

### Railway
```bash
# Auto-deploys from GitHub
# Add PostgreSQL with one click
```
**Cost**: $5/mo starter + $10/mo for DB

### DigitalOcean
```bash
# Use App Platform
# Connect GitHub repo
# Add managed database
```
**Cost**: ~$27/mo (app + database)

## Troubleshooting

### Port Already in Use
```bash
# Change port in .env
PORT=3001

# Or kill existing process
lsof -ti:5173 | xargs kill -9
```

### Build Errors
```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Clear build cache
rm -rf build/
npm run build
```

### Database Connection
```bash
# Test connection
psql $DATABASE_URL

# Check environment variable
echo $DATABASE_URL
```

### API Rate Limits
- **GitHub without token**: 60 requests/hour
- **GitHub with token**: 5,000 requests/hour
- **Stack Overflow**: 300 requests/day without key

**Solution**: Add your API tokens

### Type Errors
```bash
# Check all type errors
npm run typecheck

# Auto-fix some issues
npm run typecheck --fix
```

## Performance Tips

### Development
- Use React DevTools Profiler
- Enable source maps
- Use browser DevTools Performance tab

### Production
- Enable caching headers
- Use CDN for static assets
- Implement Redis caching
- Add database indexes
- Enable gzip compression

## Security Checklist

- [ ] Strong JWT_SECRET (32+ characters)
- [ ] No secrets in code/git
- [ ] HTTPS enabled
- [ ] CORS configured
- [ ] Rate limiting active
- [ ] Input validation
- [ ] SQL injection prevention
- [ ] XSS protection

## Monitoring

### Health Check
```bash
curl https://your-domain.com/api/health
```

### View Logs (Self-hosted)
```bash
# PM2 logs
pm2 logs devhub

# Nginx logs
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### Database Stats
```sql
-- Connection count
SELECT count(*) FROM pg_stat_activity;

-- Table sizes
SELECT 
  schemaname,
  tablename,
  pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

## Quick Wins

### Performance
1. Add `loading="lazy"` to images
2. Implement code splitting
3. Enable compression
4. Add service worker
5. Use image CDN

### Features
1. Add dark mode toggle
2. Implement search
3. Add export functionality
4. Email notifications
5. Social sharing

### UX
1. Add loading skeletons
2. Improve error messages
3. Add tooltips
4. Keyboard shortcuts
5. Better mobile navigation

## Resources

- [React Router Docs](https://reactrouter.com)
- [Radix UI](https://www.radix-ui.com/)
- [GitHub API Docs](https://docs.github.com/en/rest)
- [Stack Exchange API](https://api.stackexchange.com/docs)
- [Vercel Docs](https://vercel.com/docs)

## Support

**Found a bug?**
1. Check console for errors
2. Review logs
3. Search existing issues
4. Create detailed bug report

**Need a feature?**
1. Check roadmap
2. Create feature request
3. Consider contributing
4. Vote on existing requests

**Getting errors?**
1. Check environment variables
2. Verify API keys
3. Test with sample data
4. Review error logs
5. Check network requests

---

**Pro Tip**: Bookmark this page for quick access to commands and troubleshooting steps!
