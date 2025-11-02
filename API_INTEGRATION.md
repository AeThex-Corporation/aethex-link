# API Integration Guide

This guide explains how to integrate external APIs and extend DevHub with new data sources.

## Current Integrations

### GitHub API

**Library**: `@octokit/rest`  
**Documentation**: https://octokit.github.io/rest.js/

#### Setup
```typescript
import { Octokit } from '@octokit/rest';

const octokit = new Octokit({
  auth: process.env.GITHUB_TOKEN
});
```

#### Available Methods
- `getProfile(username)` - Fetch user profile
- `getRepositories(username)` - Get user repositories
- `getActivity(username)` - Fetch recent activity
- `getContributions(username)` - Calculate contributions
- `calculateSkillScores(username)` - Generate skill scores from languages

#### Rate Limits
- Unauthenticated: 60 requests/hour
- Authenticated: 5,000 requests/hour

### Stack Overflow API

**Documentation**: https://api.stackexchange.com/docs

#### Setup
```typescript
import axios from 'axios';

const API_URL = 'https://api.stackexchange.com/2.3';
const API_KEY = process.env.STACKOVERFLOW_API_KEY;
```

#### Available Methods
- `getUser(userId)` - Fetch user profile
- `getAnswers(userId)` - Get user answers
- `getQuestions(userId)` - Get user questions
- `getActivity(userId)` - Combined activity
- `getTopTags(userId)` - Most used tags

#### Rate Limits
- No key: 300 requests/day
- With key: 10,000 requests/day

## Adding New Integrations

### Step 1: Create Service Class

Create a new file in `app/lib/`:

```typescript
// app/lib/gitlab.server.ts
import axios from 'axios';

export interface GitLabProfile {
  id: number;
  username: string;
  name: string;
  avatar: string;
  bio: string;
}

export class GitLabService {
  private apiUrl = 'https://gitlab.com/api/v4';
  private token: string;

  constructor(token?: string) {
    this.token = token || process.env.GITLAB_TOKEN || '';
  }

  async getProfile(username: string): Promise<GitLabProfile> {
    const response = await axios.get(
      `${this.apiUrl}/users?username=${username}`,
      {
        headers: this.token ? { 'PRIVATE-TOKEN': this.token } : {},
      }
    );

    const user = response.data[0];
    
    return {
      id: user.id,
      username: user.username,
      name: user.name,
      avatar: user.avatar_url,
      bio: user.bio || '',
    };
  }

  async getProjects(userId: number) {
    const response = await axios.get(
      `${this.apiUrl}/users/${userId}/projects`,
      {
        headers: this.token ? { 'PRIVATE-TOKEN': this.token } : {},
      }
    );

    return response.data;
  }

  async calculateSkillScores(userId: number): Promise<Record<string, number>> {
    const projects = await this.getProjects(userId);
    const skills: Record<string, number> = {};

    for (const project of projects) {
      const languages = project.languages || {};
      for (const [lang, percentage] of Object.entries(languages)) {
        skills[lang] = (skills[lang] || 0) + (percentage as number);
      }
    }

    // Normalize scores
    const maxScore = Math.max(...Object.values(skills));
    for (const lang in skills) {
      skills[lang] = Math.min(100, Math.round((skills[lang] / maxScore) * 100));
    }

    return skills;
  }
}
```

### Step 2: Update Database Schema

Add new field to profiles table:

```sql
ALTER TABLE profiles ADD COLUMN gitlab_username VARCHAR(100);
```

Update TypeScript interface:

```typescript
// app/lib/db.server.ts
export interface UserProfile {
  userId: string;
  githubUsername?: string;
  stackOverflowId?: string;
  gitlabUsername?: string; // New field
  blogUrl?: string;
  npmUsername?: string;
  lastSyncedAt?: Date;
}
```

### Step 3: Update Aggregation Service

```typescript
// app/lib/aggregation.server.ts
import { GitLabService } from './gitlab.server';

export class AggregationService {
  private gitlabService: GitLabService;

  constructor(githubToken?: string, gitlabToken?: string) {
    // ... existing code
    this.gitlabService = new GitLabService(gitlabToken);
  }

  async aggregateUserProfile(userId: string) {
    const profile = await db.profile.findByUserId(userId);
    if (!profile) throw new Error('Profile not found');

    const results = await Promise.allSettled([
      // ... existing integrations
      profile.gitlabUsername 
        ? this.aggregateGitLab(profile.gitlabUsername) 
        : Promise.resolve(null),
    ]);

    const gitlabData = results[2].status === 'fulfilled' ? results[2].value : null;

    // Merge GitLab skills
    const mergedSkills = this.mergeSkills(
      githubData?.skills || {},
      stackOverflowData?.skills || {},
      gitlabData?.skills || {}
    );

    // ... rest of aggregation
  }

  private async aggregateGitLab(username: string) {
    const [profile, projects, skills] = await Promise.all([
      this.gitlabService.getProfile(username),
      this.gitlabService.getProjects(profile.id),
      this.gitlabService.calculateSkillScores(profile.id),
    ]);

    return { profile, projects, skills };
  }

  private mergeSkills(
    githubSkills: Record<string, number>,
    stackOverflowSkills: Record<string, number>,
    gitlabSkills: Record<string, number>
  ) {
    const allSkills = new Set([
      ...Object.keys(githubSkills),
      ...Object.keys(stackOverflowSkills),
      ...Object.keys(gitlabSkills),
    ]);

    const merged = [];

    for (const skill of allSkills) {
      const githubScore = githubSkills[skill] || 0;
      const stackOverflowScore = stackOverflowSkills[skill] || 0;
      const gitlabScore = gitlabSkills[skill] || 0;

      // Weighted average (adjust weights as needed)
      const level = Math.round(
        githubScore * 0.4 + 
        stackOverflowScore * 0.3 + 
        gitlabScore * 0.3
      );

      merged.push({
        name: skill,
        level,
        verified: true,
        category: this.categorizeSkill(skill),
        trend: this.calculateTrend(skill, level),
        sources: {
          github: githubScore,
          stackoverflow: stackOverflowScore,
          gitlab: gitlabScore,
        },
      });
    }

    return merged.sort((a, b) => b.level - a.level);
  }
}
```

### Step 4: Update API Route

```typescript
// app/routes/api.profile.sync.ts
export async function action({ request }: Route.ActionArgs) {
  const user = await auth.requireAuth(request);
  const body = await request.json();

  const { 
    githubUsername, 
    stackOverflowId, 
    gitlabUsername, // New field
    githubToken,
    gitlabToken // New field
  } = body;

  // Update profile
  await db.profile.update(user.id, {
    githubUsername: githubUsername || undefined,
    stackOverflowId: stackOverflowId || undefined,
    gitlabUsername: gitlabUsername || undefined,
  });

  // Aggregate with new service
  const aggregationService = new AggregationService(githubToken, gitlabToken);
  const profile = await aggregationService.aggregateUserProfile(user.id);

  return Response.json({ success: true, profile });
}
```

### Step 5: Update UI Components

```typescript
// app/components/profile-sync-dialog/profile-sync-dialog.tsx
export function ProfileSyncDialog({ trigger, onSuccess }: ProfileSyncDialogProps) {
  const [gitlabUsername, setGitlabUsername] = useState('');

  const handleSync = async (e: React.FormEvent) => {
    e.preventDefault();
    
    await syncProfile({
      githubUsername: githubUsername || undefined,
      stackOverflowId: stackOverflowId || undefined,
      gitlabUsername: gitlabUsername || undefined, // New field
    });
    
    setOpen(false);
    onSuccess?.();
  };

  return (
    // Add GitLab section to form
    <div className={styles.section}>
      <div className={styles.sectionHeader}>
        <GitlabIcon size={20} />
        <h3>GitLab</h3>
      </div>
      <div className={styles.field}>
        <Label htmlFor="gitlab-username">GitLab Username</Label>
        <Input
          id="gitlab-username"
          value={gitlabUsername}
          onChange={(e) => setGitlabUsername(e.target.value)}
          placeholder="your-username"
        />
      </div>
    </div>
  );
}
```

## Integration Checklist

When adding a new integration:

- [ ] Create service class in `app/lib/`
- [ ] Define TypeScript interfaces
- [ ] Implement data fetching methods
- [ ] Implement skill calculation
- [ ] Update database schema
- [ ] Update aggregation service
- [ ] Create API endpoints
- [ ] Update UI components
- [ ] Add error handling
- [ ] Implement rate limiting
- [ ] Add caching if needed
- [ ] Update documentation
- [ ] Add tests

## Common Integration Patterns

### OAuth Authentication Flow

```typescript
// app/routes/api.auth.oauth.callback.ts
export async function loader({ request }: Route.LoaderArgs) {
  const url = new URL(request.url);
  const code = url.searchParams.get('code');

  if (!code) {
    throw new Response('Missing code', { status: 400 });
  }

  // Exchange code for access token
  const tokenResponse = await fetch('https://oauth-provider.com/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      code,
      client_id: process.env.OAUTH_CLIENT_ID,
      client_secret: process.env.OAUTH_CLIENT_SECRET,
      redirect_uri: process.env.OAUTH_REDIRECT_URI,
    }),
  });

  const { access_token } = await tokenResponse.json();

  // Store token in database
  await db.user.update(userId, {
    oauthToken: access_token,
  });

  return redirect('/dashboard');
}
```

### Webhook Integration

```typescript
// app/routes/api.webhooks.github.ts
import crypto from 'crypto';

export async function action({ request }: Route.ActionArgs) {
  const signature = request.headers.get('X-Hub-Signature-256');
  const body = await request.text();

  // Verify webhook signature
  const hmac = crypto.createHmac('sha256', process.env.GITHUB_WEBHOOK_SECRET);
  const digest = 'sha256=' + hmac.update(body).digest('hex');

  if (signature !== digest) {
    throw new Response('Invalid signature', { status: 401 });
  }

  const payload = JSON.parse(body);

  // Process webhook event
  if (payload.action === 'created' && payload.comment) {
    // Handle new comment
    await handleNewComment(payload);
  }

  return Response.json({ received: true });
}
```

### Rate Limiting

```typescript
// app/lib/rate-limiter.server.ts
import Redis from 'ioredis';

const redis = new Redis(process.env.REDIS_URL);

export async function checkRateLimit(
  userId: string,
  endpoint: string,
  limit: number = 100,
  window: number = 3600 // 1 hour
): Promise<boolean> {
  const key = `ratelimit:${userId}:${endpoint}`;
  const current = await redis.incr(key);

  if (current === 1) {
    await redis.expire(key, window);
  }

  return current <= limit;
}
```

### Caching Strategy

```typescript
// app/lib/cache.server.ts
import Redis from 'ioredis';

const redis = new Redis(process.env.REDIS_URL);

export async function getCached<T>(
  key: string,
  fetcher: () => Promise<T>,
  ttl: number = 3600
): Promise<T> {
  // Try cache first
  const cached = await redis.get(key);
  if (cached) {
    return JSON.parse(cached);
  }

  // Fetch fresh data
  const data = await fetcher();

  // Cache for next time
  await redis.setex(key, ttl, JSON.stringify(data));

  return data;
}

// Usage
const profile = await getCached(
  `profile:${userId}`,
  () => aggregationService.aggregateUserProfile(userId),
  3600 // 1 hour
);
```

## Error Handling

```typescript
export class APIError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public provider: string
  ) {
    super(message);
    this.name = 'APIError';
  }
}

export async function handleAPIRequest<T>(
  request: () => Promise<T>,
  provider: string
): Promise<T> {
  try {
    return await request();
  } catch (error: any) {
    if (error.response) {
      throw new APIError(
        error.response.data.message || 'API request failed',
        error.response.status,
        provider
      );
    }
    throw error;
  }
}
```

## Testing Integrations

```typescript
// tests/github.test.ts
import { describe, it, expect, vi } from 'vitest';
import { GitHubService } from '../app/lib/github.server';

describe('GitHubService', () => {
  it('fetches user profile', async () => {
    const service = new GitHubService('fake-token');
    
    // Mock the API call
    vi.spyOn(service, 'getProfile').mockResolvedValue({
      username: 'testuser',
      name: 'Test User',
      bio: 'Test bio',
      avatar: 'https://example.com/avatar.jpg',
      location: 'Test City',
      publicRepos: 10,
      followers: 100,
      following: 50,
      createdAt: '2020-01-01T00:00:00Z',
    });

    const profile = await service.getProfile('testuser');
    
    expect(profile.username).toBe('testuser');
    expect(profile.publicRepos).toBe(10);
  });
});
```

## Best Practices

1. **Always handle rate limits** - Implement exponential backoff
2. **Cache aggressively** - External APIs are slow
3. **Fail gracefully** - Don't crash if one integration fails
4. **Validate data** - External data can be malformed
5. **Log errors** - Track integration issues
6. **Use TypeScript** - Type safety prevents bugs
7. **Document everything** - Make it easy for others
8. **Test thoroughly** - Mock API responses
9. **Monitor usage** - Track API quota
10. **Secure credentials** - Never commit tokens

## Resources

- [GitHub API Docs](https://docs.github.com/en/rest)
- [Stack Exchange API Docs](https://api.stackexchange.com/docs)
- [GitLab API Docs](https://docs.gitlab.com/ee/api/)
- [npm Registry API](https://github.com/npm/registry/blob/master/docs/REGISTRY-API.md)
- [Dev.to API](https://developers.forem.com/api)
