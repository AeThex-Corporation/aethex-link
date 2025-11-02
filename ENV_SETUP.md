# Environment Variables Setup

Create a `.env` file in the root directory with the following variables:

```env
# Authentication
# Generate a strong random secret for production
JWT_SECRET=your-secret-key-change-in-production

# GitHub API
# Create a personal access token at https://github.com/settings/tokens
# Scopes needed: public_repo, read:user
GITHUB_TOKEN=ghp_your_github_personal_access_token

# Stack Overflow API
# Register an app at https://stackapps.com/apps/oauth/register
STACKOVERFLOW_API_KEY=your_stackoverflow_api_key

# Database (for production)
# DATABASE_URL=postgresql://user:password@localhost:5432/devhub

# Redis (for production caching)
# REDIS_URL=redis://localhost:6379

# Node Environment
NODE_ENV=development
```

## How to Get API Keys

### GitHub Personal Access Token
1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Give it a descriptive name
4. Select scopes: `public_repo`, `read:user`
5. Click "Generate token"
6. Copy the token to your `.env` file

### Stack Overflow API Key
1. Go to https://stackapps.com/apps/oauth/register
2. Fill in the application details
3. Get your API key
4. Add it to your `.env` file

**Note**: The platform will work with public GitHub data without a token, but you'll have lower rate limits.
