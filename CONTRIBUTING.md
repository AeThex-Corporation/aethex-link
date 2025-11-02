# Contributing to DevHub

Thank you for your interest in contributing to DevHub! This document provides guidelines and instructions for contributing.

## Code of Conduct

Be respectful, inclusive, and professional. We're all here to build something great together.

## Getting Started

1. Fork the repository
2. Clone your fork
3. Create a new branch for your feature
4. Make your changes
5. Submit a pull request

## Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/devhub.git
cd devhub

# Install dependencies
npm install

# Start development server
npm run dev
```

## Project Structure

```
app/
├── components/          # React components
│   ├── ui/             # Reusable UI components
│   └── [feature]/      # Feature-specific components
├── hooks/              # Custom React hooks
├── lib/                # Server-side libraries
│   ├── *.server.ts     # Server-only code
│   └── *.client.ts     # Client-only code
├── routes/             # Route handlers and pages
├── styles/             # Global styles and themes
└── data/               # Mock data and fixtures

```

## Coding Standards

### TypeScript

- Use TypeScript for all new files
- Enable strict mode
- Add proper type annotations
- Avoid `any` types when possible

### React

- Use functional components
- Prefer hooks over class components
- Keep components small and focused
- Extract reusable logic into custom hooks

### CSS

- Use CSS Modules for component styles
- Follow existing naming conventions
- Use CSS variables for theming
- Ensure responsive design

### Code Style

- Use 2 spaces for indentation
- Use semicolons
- Use single quotes for strings
- Maximum line length: 120 characters

## Component Guidelines

### Creating a New Component

```typescript
// app/components/my-component/my-component.tsx
import styles from './my-component.module.css';

interface MyComponentProps {
  title: string;
  onAction?: () => void;
  className?: string;
}

export function MyComponent({ title, onAction, className }: MyComponentProps) {
  return (
    <div className={`${styles.container} ${className || ''}`}>
      <h2>{title}</h2>
      {onAction && <button onClick={onAction}>Action</button>}
    </div>
  );
}
```

```css
/* app/components/my-component/my-component.module.css */
.container {
  padding: var(--space-4);
  border-radius: var(--radius-2);
}
```

## Backend Guidelines

### Creating a New Service

```typescript
// app/lib/my-service.server.ts

export class MyService {
  constructor(private apiKey: string) {}

  async fetchData(id: string) {
    // Implementation
  }
}
```

### Creating a New API Route

```typescript
// app/routes/api.my-endpoint.ts
import type { Route } from './+types/api.my-endpoint';
import { auth } from '~/lib/auth.server';

export async function loader({ request }: Route.LoaderArgs) {
  const user = await auth.requireAuth(request);
  
  // Implementation
  
  return Response.json({ data });
}

export async function action({ request }: Route.ActionArgs) {
  const user = await auth.requireAuth(request);
  const body = await request.json();
  
  // Implementation
  
  return Response.json({ success: true });
}
```

## Testing

### Unit Tests (Future)

```typescript
import { describe, it, expect } from 'vitest';
import { MyComponent } from './my-component';

describe('MyComponent', () => {
  it('renders correctly', () => {
    // Test implementation
  });
});
```

### Integration Tests (Future)

```typescript
import { test, expect } from '@playwright/test';

test('user can login', async ({ page }) => {
  await page.goto('/auth');
  await page.fill('input[name="email"]', 'test@example.com');
  await page.fill('input[name="password"]', 'password');
  await page.click('button[type="submit"]');
  
  await expect(page).toHaveURL('/dashboard');
});
```

## Commit Guidelines

### Commit Message Format

```
type(scope): subject

body

footer
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

### Examples

```
feat(auth): add GitHub OAuth integration

- Implement OAuth flow
- Add GitHub profile sync
- Update user model

Closes #123
```

```
fix(dashboard): correct analytics calculation

The profile views were counting duplicates. This fix ensures
unique visitors are counted correctly.
```

## Pull Request Process

1. **Update your branch**
   ```bash
   git checkout main
   git pull upstream main
   git checkout your-feature-branch
   git rebase main
   ```

2. **Ensure code quality**
   ```bash
   npm run typecheck
   npm run build
   ```

3. **Create pull request**
   - Write clear title and description
   - Link related issues
   - Add screenshots for UI changes
   - Request review from maintainers

4. **Respond to feedback**
   - Address review comments
   - Make requested changes
   - Push updates to your branch

5. **Merge**
   - Maintainers will merge when approved
   - Delete your branch after merge

## Feature Requests

### Before Submitting

- Check if feature already exists
- Search existing issues
- Gather community feedback

### Submitting a Feature Request

Create an issue with:
- Clear use case
- Expected behavior
- Alternatives considered
- Mockups/examples (if UI change)

## Bug Reports

### Before Submitting

- Update to latest version
- Check existing issues
- Verify it's actually a bug

### Submitting a Bug Report

Create an issue with:
- DevHub version
- Browser/Node.js version
- Steps to reproduce
- Expected behavior
- Actual behavior
- Screenshots/logs

## Areas for Contribution

### High Priority
- PostgreSQL/MongoDB integration
- Redis caching layer
- Comprehensive test suite
- API documentation
- Mobile responsiveness improvements

### Medium Priority
- Additional platform integrations (GitLab, Bitbucket)
- Advanced analytics features
- AI-powered recommendations
- Real-time notifications

### Good First Issues
- UI/UX improvements
- Documentation updates
- Bug fixes
- Accessibility enhancements

## Code Review Guidelines

### For Reviewers
- Be constructive and respectful
- Focus on code quality, not style preferences
- Suggest improvements, don't demand
- Approve if changes are good enough

### For Contributors
- Respond to all comments
- Ask questions if unclear
- Don't take feedback personally
- Learn from the review

## Documentation

### Updating Documentation
- Keep README.md current
- Update API documentation
- Add JSDoc comments to functions
- Include examples

### Documentation Style
- Use clear, simple language
- Include code examples
- Add diagrams where helpful
- Keep it concise

## Community

### Communication Channels
- GitHub Issues: Bug reports and features
- GitHub Discussions: Questions and ideas
- Discord: Real-time chat (coming soon)

### Getting Help
- Check documentation first
- Search existing issues
- Ask in discussions
- Be specific and provide context

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Recognition

Contributors will be:
- Added to CONTRIBUTORS.md
- Mentioned in release notes
- Appreciated by the community!

Thank you for contributing to DevHub! 🎉
