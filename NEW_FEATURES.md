# New Features Documentation

This document outlines the newly implemented features for the Aethex platform.

## 🚀 New Platform Integrations

### 1. NPM (Node Package Manager)
- **Service**: `app/lib/npm.server.ts`
- **Features**:
  - Fetch user's published packages
  - Get download statistics
  - Track weekly downloads
  - Display package quality scores

### 2. PyPI (Python Package Index)
- **Service**: `app/lib/pypi.server.ts`
- **Features**:
  - Fetch Python packages by username
  - Get package metadata
  - Track releases and versions

### 3. GitLab
- **Service**: `app/lib/gitlab.server.ts`
- **Features**:
  - Fetch user profile and projects
  - Get repository statistics (stars, forks)
  - Track contribution activity
  - Support for private GitLab instances with tokens

### 4. Dev.to
- **Service**: `app/lib/devto.server.ts`
- **Features**:
  - Fetch user articles
  - Get reaction counts and comments
  - Track reading time and engagement
  - Display cover images

### 5. Medium
- **Service**: `app/lib/medium.server.ts`
- **Features**:
  - Parse RSS feed for articles
  - Extract article metadata
  - Get publication categories
  - Display thumbnails

## 🌐 Subdomain Routing

### Custom Portfolio Pages
- **Route**: `app/routes/subdomain.$username.tsx`
- **Features**:
  - Custom subdomain for each user (username.aethex.com)
  - Beautiful portfolio showcase
  - Automatic profile synchronization
  - Responsive design
  - SEO-optimized

### How It Works:
```
https://johndoe.aethex.com → User's custom portfolio
https://aethex.com/johndoe → Also accessible via path
```

## 🛒 Domain Marketplace

### Buy & Sell .aethex Domains
- **Route**: `app/routes/marketplace.tsx`
- **API**: `app/routes/api.marketplace.ts`

### Features:
- List domains for sale
- Browse active listings
- Search functionality
- Secure domain transfers
- Transaction history
- Seller profiles
- Price in USD/crypto

### Usage:
1. Navigate to `/marketplace`
2. Click "List Your Domain" to sell
3. Browse listings and click "Buy Domain" to purchase
4. Domain ownership transfers automatically

## 👥 Team Collaboration

### Multi-User Teams
- **Route**: `app/routes/teams.tsx`
- **API**: `app/routes/api.teams.ts`

### Features:
- Create and manage teams
- Role-based permissions (Owner, Admin, Member)
- Invite team members
- Team projects and portfolios
- Collaboration spaces

### Roles:
- **Owner**: Full control, can delete team
- **Admin**: Can invite members, manage settings
- **Member**: Can view and collaborate

## 🎯 Recruiter Search & Matching

### Talent Discovery
- **API**: `app/routes/api.recruiters.ts`

### Features:
- Recruiter profiles
- Company verification
- Candidate search by skills
- Location-based filtering
- Direct messaging
- Job posting integration

### Recruiter Profile:
```typescript
{
  company: string;
  position: string;
  lookingFor: string[]; // Skills
  industries: string[];
  verified: boolean;
}
```

## 🔌 Public API for Third-Party Integrations

### RESTful API
- **Endpoint**: `/api/public/profile/:username`
- **Auth**: API key via `X-API-Key` header

### Features:
- Rate limiting
- Key permissions system
- Expiration dates
- Usage tracking
- CORS support

### Creating API Keys:
1. Navigate to Settings → API Keys
2. Click "Create New Key"
3. Set permissions (profile:read, portfolio:read, etc.)
4. Optional: Set expiration date
5. Copy key (shown only once)

### Example Request:
```bash
curl https://aethex.com/api/public/profile/johndoe \
  -H "X-API-Key: aethex_abc123..."
```

### Example Response:
```json
{
  "profile": {
    "id": "...",
    "username": "johndoe",
    "name": "John Doe",
    "bio": "Full-stack developer",
    "platforms": {
      "github": "johndoe",
      "npm": "johndoe",
      "devto": "johndoe"
    },
    "portfolio": [...]
  }
}
```

## 🎨 Embeddable Widgets

### Share Your Profile Anywhere
- **Route**: `app/routes/widgets.tsx`
- **Generator**: `app/routes/api.widget.embed.ts`

### Widget Types:
1. **Profile Card** - Full profile with avatar, bio, and links
2. **Simple Badge** - Compact badge with essential info
3. **Portfolio Grid** - Showcase recent projects

### Themes:
- Light mode
- Dark mode

### Usage:
1. Navigate to `/widgets`
2. Select widget type and theme
3. Copy generated embed code
4. Paste into your website's HTML

### Embed Code Example:
```html
<!-- Aethex Profile Widget -->
<div id="aethex-widget"></div>
<script src="https://aethex.com/api/widget/embed?username=johndoe&type=card&theme=light"></script>
```

### Widget Features:
- Auto-syncs with profile
- Responsive design
- No dependencies
- Lightweight (~5KB)
- SEO-friendly
- Customizable

## 📊 Database Schema Updates

### New Tables/Collections:
- `Team` - Team information
- `TeamMember` - Team memberships with roles
- `MarketplaceListing` - Domain listings for sale
- `RecruiterProfile` - Recruiter information
- `ApiKey` - API authentication keys

### Updated Tables:
- `UserProfile` - Added new platform fields:
  - `npmUsername`
  - `pypiUsername`
  - `gitlabUsername`
  - `gitlabToken`
  - `devtoUsername`
  - `devtoApiKey`
  - `mediumUsername`

## 🔐 API Permissions System

### Available Permissions:
- `profile:read` - Read user profiles
- `profile:write` - Update profiles
- `portfolio:read` - Read portfolio items
- `portfolio:write` - Create/update portfolio
- `analytics:read` - Access analytics data
- `teams:read` - View team information
- `teams:write` - Manage teams

## 🚦 Rate Limiting

All public API endpoints implement rate limiting:
- 100 requests per minute per API key
- 1000 requests per hour per API key
- Sliding window algorithm

## 📈 Analytics Tracking

New events tracked:
- `marketplace_listing_created`
- `marketplace_purchase`
- `team_created`
- `team_member_invited`
- `api_key_created`
- `widget_embedded`
- `platform_synced` (for each new platform)

## 🔄 Integration Flow

### Syncing New Platforms:
```typescript
import { api } from '~/lib/api.client';

await api.syncProfile({
  npmUsername: 'johndoe',
  gitlabUsername: 'johndoe',
  gitlabToken: 'glpat-...',
  devtoUsername: 'johndoe',
  mediumUsername: '@johndoe',
});
```

## 🎯 Future Enhancements

Potential additions:
- WebSocket support for real-time updates
- GraphQL API alternative
- Webhook notifications
- Advanced search filters
- Team portfolios
- Recruiter analytics dashboard
- Domain auctions
- Escrow service for domain sales

## 📝 Notes

- All new features are backward compatible
- Existing user data remains unchanged
- API versioning planned for v2
- Performance optimized for scale
- Security audited

## 🤝 Contributing

To add new platform integrations:
1. Create service in `app/lib/[platform].server.ts`
2. Update database schema in `app/lib/db.server.ts`
3. Add API route in `app/routes/api.platforms.sync.ts`
4. Update client API in `app/lib/api.client.ts`
5. Add UI components as needed

---

For questions or support, please open an issue on GitHub.
