# Aethex Public API Documentation

## Overview

The Aethex Public API allows third-party developers to integrate Aethex profiles and data into their applications. All API endpoints require authentication via API keys.

## Base URL

```
https://aethex.com/api
```

## Authentication

Include your API key in the request header:

```
X-API-Key: aethex_your_api_key_here
```

### Creating an API Key

1. Log in to your Aethex account
2. Navigate to Settings → API Keys
3. Click "Create New Key"
4. Set permissions and optional expiration
5. Copy the key (shown only once)

## Rate Limits

- **100 requests per minute** per API key
- **1,000 requests per hour** per API key
- **10,000 requests per day** per API key

Rate limit headers included in response:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640000000
```

## Endpoints

### Get User Profile

Retrieve a user's public profile information.

**Endpoint:** `GET /public/profile/:username`

**Parameters:**
- `username` (required) - The user's Aethex username

**Response:**
```json
{
  "profile": {
    "id": "user_123",
    "username": "johndoe",
    "name": "John Doe",
    "bio": "Full-stack developer and creator",
    "avatar": "https://example.com/avatar.jpg",
    "location": "San Francisco, CA",
    "title": "Senior Developer",
    "aethexDomain": "johndoe",
    "platforms": {
      "github": "johndoe",
      "stackoverflow": "123456",
      "npm": "johndoe",
      "gitlab": "johndoe",
      "devto": "johndoe",
      "medium": "@johndoe"
    },
    "portfolio": [
      {
        "id": "portfolio_1",
        "type": "project",
        "title": "Awesome App",
        "description": "A revolutionary application",
        "platform": "GitHub",
        "url": "https://github.com/johndoe/awesome-app",
        "thumbnailUrl": "https://example.com/thumb.jpg",
        "tags": ["react", "typescript", "node"],
        "stats": {
          "stars": 1500,
          "forks": 200
        }
      }
    ]
  }
}
```

**Example Request:**
```bash
curl -H "X-API-Key: aethex_abc123..." \
  https://aethex.com/api/public/profile/johndoe
```

**Error Responses:**

- `401 Unauthorized` - Invalid or missing API key
```json
{
  "error": "Invalid API key"
}
```

- `403 Forbidden` - Insufficient permissions
```json
{
  "error": "Insufficient permissions"
}
```

- `404 Not Found` - User not found
```json
{
  "error": "User not found"
}
```

- `429 Too Many Requests` - Rate limit exceeded
```json
{
  "error": "Rate limit exceeded",
  "retryAfter": 60
}
```

## Embeddable Widgets

### Widget Embed Script

Generate an embeddable widget for any website.

**Endpoint:** `GET /widget/embed`

**Query Parameters:**
- `username` (required) - Aethex username
- `type` (optional) - Widget type: `card`, `badge`, `portfolio` (default: `card`)
- `theme` (optional) - Theme: `light`, `dark` (default: `light`)

**Response:** JavaScript code

**Example:**
```html
<div id="aethex-widget"></div>
<script src="https://aethex.com/api/widget/embed?username=johndoe&type=card&theme=dark"></script>
```

## Permissions

API keys can have the following permissions:

- `profile:read` - Read user profiles
- `profile:write` - Update user profiles (requires user authorization)
- `portfolio:read` - Read portfolio items
- `portfolio:write` - Create/update portfolio items
- `analytics:read` - Access analytics data
- `teams:read` - View team information
- `teams:write` - Manage team memberships

## Webhooks (Coming Soon)

Subscribe to events:
- `profile.updated` - User profile changed
- `portfolio.created` - New portfolio item added
- `domain.registered` - New domain registered
- `team.member.added` - New team member joined

## SDKs

### JavaScript/TypeScript

```bash
npm install @aethex/sdk
```

```typescript
import { AethexClient } from '@aethex/sdk';

const client = new AethexClient({
  apiKey: 'aethex_your_api_key',
});

const profile = await client.getProfile('johndoe');
console.log(profile);
```

### Python

```bash
pip install aethex-sdk
```

```python
from aethex import AethexClient

client = AethexClient(api_key='aethex_your_api_key')
profile = client.get_profile('johndoe')
print(profile)
```

## Best Practices

### Caching

Cache responses to reduce API calls:

```javascript
const cache = new Map();
const CACHE_TTL = 3600000; // 1 hour

async function getCachedProfile(username) {
  const cached = cache.get(username);
  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }

  const data = await client.getProfile(username);
  cache.set(username, { data, timestamp: Date.now() });
  return data;
}
```

### Error Handling

Always handle errors gracefully:

```javascript
try {
  const profile = await client.getProfile(username);
  return profile;
} catch (error) {
  if (error.status === 404) {
    console.log('User not found');
  } else if (error.status === 429) {
    console.log('Rate limit exceeded, retry after:', error.retryAfter);
  } else {
    console.error('API error:', error);
  }
  return null;
}
```

### Pagination

For endpoints returning lists:

```javascript
let page = 1;
const allItems = [];

while (true) {
  const response = await client.getPortfolio(username, { page, limit: 100 });
  allItems.push(...response.items);
  
  if (!response.hasMore) break;
  page++;
}
```

## Support

- **Documentation**: https://docs.aethex.com
- **GitHub Issues**: https://github.com/aethex/api/issues
- **Email**: api-support@aethex.com
- **Discord**: https://discord.gg/aethex

## Changelog

### Version 1.0.0 (2024-01-15)
- Initial public API release
- Profile and portfolio endpoints
- Widget embed functionality
- API key management

### Upcoming Features
- GraphQL API
- Webhook subscriptions
- Batch operations
- Advanced filtering
- Real-time updates via WebSockets

## Terms of Service

By using the Aethex API, you agree to:
- Not exceed rate limits
- Cache responses appropriately
- Respect user privacy
- Attribute Aethex when displaying user data
- Not reverse engineer the API

See full [Terms of Service](https://aethex.com/terms) for details.
