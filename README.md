# CreatorHub - Multi-Platform Creator Verification

A comprehensive verification platform for game developers, AI engineers, and software developers. Automatically aggregates and verifies your work from Roblox, Unity, Unreal Engine, Meta Horizon Worlds, GitHub, Hugging Face, Kaggle, and 12+ other platforms to build your complete professional profile.

## Features

### 🎮 Game Development Verification
- Track game releases, player counts, and revenue across platforms
- Verify Roblox game statistics and Lua scripting skills
- Unity and Unreal Engine project portfolios
- Meta Horizon Worlds VR experience metrics
- Asset Store and Marketplace sales tracking

### 🧠 AI Engineering Verification
- Hugging Face model tracking and download metrics
- Kaggle competition history and medal tracking
- Research paper citations and implementations
- Machine learning project contributions
- AI community engagement scores

### 💻 Software Development Verification
- GitHub contribution graphs and repository analytics
- Stack Overflow reputation and answer impact
- Open-source package download statistics
- Technical writing and blog engagement

### 🔐 Authentication System
- JWT-based authentication
- Secure password hashing with bcrypt
- Session management with token refresh
- Protected API routes

### 🔄 Multi-Platform Data Aggregation

**Game Development:**
- **Roblox**: Game visits, active players, Robux revenue, community ratings, Lua scripting
- **Unity**: Published games, Asset Store products, GitHub Unity projects, C# expertise
- **Unreal Engine**: Marketplace assets, Blueprint/C++ projects, community contributions
- **Meta Horizon Worlds**: VR experiences, world visitors, scripting, engagement metrics
- **Godot, GameMaker**: Project tracking and community contributions

**AI Engineering:**
- **Hugging Face**: Model uploads, downloads, community contributions
- **Kaggle**: Competition rankings, notebooks, datasets, medals
- **Papers with Code**: Research publications and implementations
- **GitHub ML**: Machine learning project contributions

**Software Development:**
- **GitHub/GitLab**: Repositories, commits, pull requests, code reviews
- **Stack Overflow**: Answers, questions, reputation, expertise tags
- **npm/PyPI**: Package authorship, downloads, maintenance activity
- **Dev.to/Medium**: Technical writing and blog posts

- **Real-time Sync**: On-demand profile synchronization from all connected platforms
- **Multi-Category Skill Calculation**: Intelligent scoring across game dev, AI, and software development

### 📊 Analytics Dashboard
- Profile view tracking
- Skill growth over time
- Source breakdown (GitHub, Stack Overflow, Blog)
- Network growth metrics
- Recruiter view tracking
- CSV data export

### 🌐 Networking Features
- High-signal connection recommendations
- Match scoring based on shared skills
- Connection request management
- Profile sharing with embeddable widgets

### ✅ Verified Credentials
- Blockchain-backed verification (planned)
- Multi-source skill validation
- Provable contribution tracking
- Transparent data sources

## Tech Stack

### Frontend
- React 19
- TypeScript
- React Router v7
- CSS Modules
- Radix UI Components
- Recharts for analytics visualization

### Backend Services
- Node.js server-side rendering
- JWT authentication
- RESTful API architecture
- Real-time data aggregation

### External APIs
- GitHub REST API (@octokit/rest)
- Stack Exchange API
- npm Registry (planned)
- Blog RSS feeds (planned)

## 🚀 Getting Started

### ✅ Setup Complete!

**The platform is fully configured and ready to run!**

```bash
npm run dev
```

Then open http://localhost:5173 and create an account!

📋 **[View Setup Status →](SETUP_COMPLETE.md)** | 📖 **[Getting Started Guide →](GET_STARTED.md)**

### Prerequisites
- Node.js 18+
- npm (included with Node.js)

### Environment Variables

Three environment variables are configured:
- **JWT_SECRET**: Authentication secret (⚠️ change for production)
- **GITHUB_TOKEN**: GitHub API access (⚠️ add your token for full functionality)
- **STACKOVERFLOW_API_KEY**: Stack Overflow API (⚠️ add for unlimited requests)

📖 **[Environment Setup Details →](ENV_SETUP.md)**

### Test Without API Keys

The platform works immediately with placeholder values:
- GitHub integration: Limited to 60 requests/hour
- Stack Overflow: Limited to 300 requests/day
- All other features: Fully functional

Try it with test accounts:
- GitHub: `octocat`
- Stack Overflow: `1` (Jon Skeet)

## Usage

### 1. Create an Account
- Navigate to `/auth`
- Sign up with your email and password
- You'll be automatically redirected to your dashboard

### 2. Connect Your Accounts
- Click "Settings" in the header
- Go to "Connections" tab
- Click "Sync Accounts"
- Enter your:
  - GitHub username
  - GitHub Personal Access Token (optional, for private contributions)
  - Stack Overflow User ID (found in your profile URL)

### 3. View Your Aggregated Profile
- Your dashboard will show:
  - Verified skills from all platforms
  - Recent contributions and activity
  - Analytics and growth metrics
  - Suggested connections

### 4. Share Your Profile
- Go to the "Share" tab
- Copy your profile link
- Embed widget on your website
- Export analytics data as CSV

## API Documentation

### Authentication

#### Register
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "name": "John Doe",
  "username": "johndoe"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword"
}
```

#### Get Current User
```http
GET /api/auth/me
Authorization: Bearer <token>
```

### Profile Management

#### Sync Profile
```http
POST /api/profile/sync
Authorization: Bearer <token>
Content-Type: application/json

{
  "githubUsername": "octocat",
  "githubToken": "ghp_...",
  "stackOverflowId": "123456"
}
```

#### Get Profile
```http
GET /api/profile/:userId
```

### Analytics

#### Get Dashboard Data
```http
GET /api/analytics/dashboard
Authorization: Bearer <token>
```

#### Export Analytics
```http
GET /api/analytics/export
Authorization: Bearer <token>
```

### Connections

#### Get Connections
```http
GET /api/connections
Authorization: Bearer <token>
```

#### Send Connection Request
```http
POST /api/connections
Authorization: Bearer <token>
Content-Type: application/json

{
  "connectedUserId": "user-id",
  "action": "request"
}
```

## Architecture

### Backend Services

#### `db.server.ts`
In-memory database abstraction (replace with PostgreSQL/MongoDB in production)
- User management
- Profile storage
- Analytics tracking
- Connection management

#### `auth.server.ts`
Authentication service
- User registration
- Login/logout
- JWT token generation and verification
- Password hashing

#### `github.server.ts`
GitHub API integration
- Profile fetching
- Repository analysis
- Contribution tracking
- Skill calculation from languages

#### `stackoverflow.server.ts`
Stack Overflow API integration
- User profile
- Answer/question aggregation
- Tag-based skill scoring
- Reputation tracking

#### `aggregation.server.ts`
Multi-platform data aggregation
- Merges data from all sources
- Calculates weighted skill scores
- Tracks contribution trends
- Generates unified profile

#### `analytics.server.ts`
Real-time analytics engine
- Event tracking
- Metric calculation
- Data visualization preparation
- CSV export

## Database Schema (Production)

When moving to production, implement these schemas:

### Users Table
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(255) NOT NULL,
  username VARCHAR(100) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  avatar VARCHAR(500),
  bio TEXT,
  location VARCHAR(255),
  title VARCHAR(255),
  github_token VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Profiles Table
```sql
CREATE TABLE profiles (
  user_id UUID PRIMARY KEY REFERENCES users(id),
  github_username VARCHAR(100),
  stackoverflow_id VARCHAR(50),
  blog_url VARCHAR(500),
  npm_username VARCHAR(100),
  last_synced_at TIMESTAMP
);
```

### Analytics Events Table
```sql
CREATE TABLE analytics_events (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  event_type VARCHAR(50) NOT NULL,
  metadata JSONB,
  timestamp TIMESTAMP DEFAULT NOW()
);
```

### Connections Table
```sql
CREATE TABLE connections (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  connected_user_id UUID REFERENCES users(id),
  status VARCHAR(20) CHECK (status IN ('pending', 'accepted', 'rejected')),
  created_at TIMESTAMP DEFAULT NOW()
);
```

## Deployment

### Environment Variables (Production)
```env
# Required
JWT_SECRET=<strong-random-secret>
DATABASE_URL=postgresql://user:password@host:5432/devhub
GITHUB_TOKEN=<your-github-token>

# Optional
STACKOVERFLOW_API_KEY=<your-stackoverflow-key>
REDIS_URL=redis://localhost:6379
NODE_ENV=production
```

### Build for Production
```bash
npm run build
npm start
```

### Docker Deployment
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

## Documentation

Comprehensive guides for getting started, testing, deployment, and future enhancements:

### Quick Start
- **[Setup Checklist](SETUP_CHECKLIST.md)** - Complete setup guide with step-by-step instructions
- **[Quick Reference](QUICK_REFERENCE.md)** - Common commands, API endpoints, and troubleshooting
- **[Environment Setup](ENV_SETUP.md)** - Configure API keys and environment variables

### Testing & Development
- **[Testing Guide](TESTING_GUIDE.md)** - Complete testing procedures for all features
- **[API Integration](API_INTEGRATION.md)** - Integration details for GitHub and Stack Overflow
- **[Implementation Summary](IMPLEMENTATION_SUMMARY.md)** - Architecture and design decisions

### Deployment & Production
- **[Production Deployment](PRODUCTION_DEPLOYMENT.md)** - Deploy to Vercel, Railway, DigitalOcean, or VPS
- **[Enhancement Roadmap](ENHANCEMENT_ROADMAP.md)** - Future features and development phases
- **[Contributing Guidelines](CONTRIBUTING.md)** - How to contribute to the project

## Roadmap

See [ENHANCEMENT_ROADMAP.md](ENHANCEMENT_ROADMAP.md) for the complete development roadmap.

### Phase 1: Core Enhancements (Weeks 1-4)
- Additional data sources (npm, GitLab, Dev.to, Medium)
- Real-time analytics with WebSockets
- Recruiter search and matching
- Payment integration (Stripe)

### Phase 2: Advanced Features (Weeks 5-8)
- Admin dashboard
- Profile customization
- Team collaboration
- Public API

### Phase 3: Platform Growth (Weeks 9-12)
- Social features
- Job board integration
- Portfolio builder
- Learning recommendations

### Phase 4: Enterprise (Weeks 13-16)
- White label solution
- Advanced security (2FA, SSO)
- Enterprise analytics

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines before submitting PRs.

## License

MIT License - see LICENSE file for details

## Security

- Never commit `.env` files
- Rotate JWT secrets regularly
- Use HTTPS in production
- Implement rate limiting
- Sanitize all user inputs
- Regular security audits

## Support

For issues and questions:
- GitHub Issues: [github.com/yourusername/devhub/issues](https://github.com/yourusername/devhub/issues)
- Email: support@devhub.dev

---

Built with ❤️ for the developer community
