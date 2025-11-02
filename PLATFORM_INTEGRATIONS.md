# Platform Integration Guide

This guide explains how to integrate additional platforms into the Aethex ecosystem.

## Supported Platforms

### Development Platforms
- **GitHub** - Repositories, contributions, stars
- **GitLab** - Projects, commits, CI/CD stats
- **Stack Overflow** - Reputation, answers, badges
- **NPM** - Package downloads, versions
- **PyPI** - Python packages, releases

### Content Platforms
- **Dev.to** - Articles, reactions, comments
- **Medium** - Blog posts, publications

### Game Development
- **Roblox** - Games, visits, Robux earnings
- **Unity Asset Store** - Assets, downloads, revenue

### AI/ML Platforms
- **Hugging Face** - Models, datasets, spaces
- **Kaggle** - Competitions, datasets, notebooks

## Adding a New Platform

### Step 1: Create Platform Service

Create a new file in `app/lib/[platform].server.ts`:

```typescript
export interface PlatformData {
  // Define your data structure
}

export class PlatformService {
  private readonly baseUrl = 'https://api.platform.com';
  private token?: string;

  constructor(token?: string) {
    this.token = token;
  }

  async getUserProfile(username: string): Promise<PlatformData> {
    // Implement API calls
    const response = await fetch(`${this.baseUrl}/users/${username}`);
    if (!response.ok) {
      throw new Error('Failed to fetch data');
    }
    return response.json();
  }
}
```

### Step 2: Update Database Schema

Add fields to `UserProfile` in `app/lib/db.server.ts`:

```typescript
export interface UserProfile {
  // ... existing fields
  platformUsername?: string;
  platformApiKey?: string;
}
```

### Step 3: Update Platform Sync Route

In `app/routes/api.platforms.sync.ts`:

```typescript
const {
  // ... existing platforms
  platformUsername,
  platformApiKey,
} = body;

await db.profile.update(user.id, {
  // ... existing updates
  platformUsername: platformUsername || undefined,
  platformApiKey: platformApiKey || undefined,
});

// Fetch platform data
if (platformUsername) {
  const platformService = new PlatformService(platformApiKey);
  const data = await platformService.getUserProfile(platformUsername);
  
  await analyticsService.trackEvent(user.id, 'platform_sync', {
    source: 'Platform Name',
    data: data,
  });
}
```

### Step 4: Update Aggregation Service

In `app/lib/aggregation.server.ts`:

```typescript
async aggregateUserProfile(userId: string) {
  const profile = await db.profile.findByUserId(userId);
  
  if (profile?.platformUsername) {
    const platformService = new PlatformService(this.platformToken);
    const platformData = await platformService.getUserProfile(profile.platformUsername);
    
    // Aggregate into unified profile
    aggregatedData.platformStats = platformData;
  }
  
  return aggregatedData;
}
```

### Step 5: Update Client API

In `app/lib/api.client.ts`:

```typescript
async syncProfile(data: {
  // ... existing platforms
  platformUsername?: string;
  platformApiKey?: string;
}) {
  // Include in sync request
}
```

### Step 6: Add UI Components

Update `app/components/profile-sync-dialog/profile-sync-dialog.tsx`:

```tsx
<div className={styles.formField}>
  <Label htmlFor="platformUsername">Platform Username</Label>
  <Input
    id="platformUsername"
    value={platformUsername}
    onChange={(e) => setPlatformUsername(e.target.value)}
    placeholder="username"
  />
</div>
```

## Platform-Specific Considerations

### API Rate Limiting
Always implement rate limiting and caching:

```typescript
class PlatformService {
  private cache = new Map<string, { data: any; timestamp: number }>();
  private readonly CACHE_TTL = 3600000; // 1 hour

  async getUserProfile(username: string) {
    const cached = this.cache.get(username);
    if (cached && Date.now() - cached.timestamp < this.CACHE_TTL) {
      return cached.data;
    }

    const data = await this.fetchUserProfile(username);
    this.cache.set(username, { data, timestamp: Date.now() });
    return data;
  }
}
```

### Authentication

#### OAuth Flow
For platforms requiring OAuth:

```typescript
export class OAuthPlatformService {
  async getAuthUrl(redirectUri: string): Promise<string> {
    return `https://platform.com/oauth/authorize?client_id=${CLIENT_ID}&redirect_uri=${redirectUri}`;
  }

  async exchangeCodeForToken(code: string): Promise<string> {
    const response = await fetch('https://platform.com/oauth/token', {
      method: 'POST',
      body: JSON.stringify({
        code,
        client_id: CLIENT_ID,
        client_secret: CLIENT_SECRET,
      }),
    });
    const data = await response.json();
    return data.access_token;
  }
}
```

#### API Keys
For API key authentication:

```typescript
private async request<T>(endpoint: string): Promise<T> {
  const headers: Record<string, string> = {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${this.apiKey}`,
  };

  const response = await fetch(`${this.baseUrl}${endpoint}`, { headers });
  return response.json();
}
```

### Error Handling

Always provide graceful fallbacks:

```typescript
try {
  const data = await platformService.getUserProfile(username);
  return data;
} catch (error) {
  console.error(`Platform sync failed:`, error);
  
  // Return partial data or cached data
  return {
    username,
    error: 'Failed to sync platform data',
    lastSynced: profile?.lastSyncedAt,
  };
}
```

### Data Normalization

Normalize platform data into common format:

```typescript
interface NormalizedProject {
  id: string;
  title: string;
  description: string;
  url: string;
  stars: number;
  language: string;
  createdAt: string;
}

function normalizeGitHubRepo(repo: GitHubRepo): NormalizedProject {
  return {
    id: repo.id.toString(),
    title: repo.name,
    description: repo.description || '',
    url: repo.html_url,
    stars: repo.stargazers_count,
    language: repo.language || 'Unknown',
    createdAt: repo.created_at,
  };
}
```

## Best Practices

### 1. Privacy & Security
- Never log sensitive tokens
- Encrypt API keys in database
- Use environment variables for secrets
- Implement token refresh logic

### 2. Performance
- Cache API responses
- Use batch requests when possible
- Implement exponential backoff
- Set appropriate timeouts

### 3. User Experience
- Show sync progress
- Display last sync time
- Allow manual refresh
- Handle disconnected states

### 4. Testing
```typescript
// Mock platform service for testing
export class MockPlatformService extends PlatformService {
  async getUserProfile(username: string) {
    return {
      username,
      stats: {
        followers: 100,
        projects: 10,
      },
    };
  }
}
```

## Example: Adding Twitter Integration

```typescript
// app/lib/twitter.server.ts
export interface TwitterProfile {
  username: string;
  name: string;
  bio: string;
  followers: number;
  tweets: number;
  verified: boolean;
}

export class TwitterService {
  private readonly baseUrl = 'https://api.twitter.com/2';
  private bearerToken: string;

  constructor(bearerToken: string) {
    this.bearerToken = bearerToken;
  }

  async getUserProfile(username: string): Promise<TwitterProfile> {
    const response = await fetch(
      `${this.baseUrl}/users/by/username/${username}?user.fields=public_metrics,description,verified`,
      {
        headers: {
          'Authorization': `Bearer ${this.bearerToken}`,
        },
      }
    );

    if (!response.ok) {
      throw new Error('Failed to fetch Twitter profile');
    }

    const data = await response.json();
    const user = data.data;

    return {
      username: user.username,
      name: user.name,
      bio: user.description,
      followers: user.public_metrics.followers_count,
      tweets: user.public_metrics.tweet_count,
      verified: user.verified,
    };
  }
}
```

## Troubleshooting

### Common Issues

1. **Rate Limiting**
   - Implement request throttling
   - Use Redis for distributed rate limiting
   - Show clear error messages to users

2. **Token Expiration**
   - Implement refresh token flow
   - Notify users when tokens expire
   - Provide re-authentication UI

3. **API Changes**
   - Version your API integrations
   - Monitor for deprecation notices
   - Implement fallback logic

## Resources

- [Platform Integration Examples](./EXAMPLES.md)
- [API Documentation](./API_DOCS.md)
- [Security Guidelines](./SECURITY.md)
