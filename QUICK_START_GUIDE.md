# Quick Start Guide: New Features

## 🚀 Getting Started

### 1. Connect New Platforms

Navigate to **Profile** → **Settings** → **Platform Connections**

#### NPM
```
Username: your-npm-username
```

#### PyPI
```
Username: your-pypi-username
```

#### GitLab
```
Username: your-gitlab-username
Access Token: glpat-xxxxx (optional, for private repos)
```

#### Dev.to
```
Username: your-devto-username
API Key: (optional, for detailed stats)
```

#### Medium
```
Username: @your-medium-username
```

### 2. Set Up Custom Portfolio Page

Your custom portfolio is automatically available at:
- `https://your-username.aethex.com`
- `https://aethex.com/your-username`

To customize:
1. Update your profile information
2. Add portfolio items
3. Your subdomain updates automatically!

### 3. List Domain on Marketplace

**Step 1**: Navigate to `/marketplace`

**Step 2**: Click "List Your Domain"

**Step 3**: Fill in details:
- Domain name (without .aethex)
- Price in USD
- Optional description

**Step 4**: Confirm listing

Your domain is now for sale!

### 4. Create a Team

**Step 1**: Navigate to `/teams`

**Step 2**: Click "Create Team"

**Step 3**: Enter:
- Team name
- Description (optional)

**Step 4**: Invite members:
- Click "Invite Member"
- Enter username
- Assign role (Admin or Member)

### 5. Generate API Key

**Step 1**: Go to Settings → API Keys

**Step 2**: Click "Create New Key"

**Step 3**: Configure:
- Name: "My App API"
- Permissions: Select what access the key needs
- Expiration: Optional (30, 90, 365 days, or never)

**Step 4**: Copy the key (shown only once!)

**Example Usage**:
```bash
curl -H "X-API-Key: aethex_your_key_here" \
  https://aethex.com/api/public/profile/johndoe
```

### 6. Embed Profile Widget

**Step 1**: Navigate to `/widgets`

**Step 2**: Choose:
- Widget Type (Card, Badge, or Portfolio)
- Theme (Light or Dark)

**Step 3**: Copy the embed code

**Step 4**: Paste into your website:
```html
<div id="aethex-widget"></div>
<script src="https://aethex.com/api/widget/embed?username=yourname&type=card&theme=light"></script>
```

## 💡 Pro Tips

### Marketplace
- Price competitively based on domain length and memorability
- Short domains (3-5 characters) typically sell for more
- Add a compelling description to attract buyers

### Teams
- Use teams for collaborative projects
- Assign Admin role to trusted members
- Teams can share portfolios (coming soon)

### API Keys
- Use different keys for different applications
- Set expiration dates for security
- Revoke keys you're no longer using
- Monitor usage in Settings

### Widgets
- Light theme works best on white/light backgrounds
- Dark theme for dark-colored sites
- Profile Card shows most information
- Badge is minimal and compact

## 🎯 Common Use Cases

### For Package Maintainers
1. Connect NPM and PyPI
2. Display download stats on profile
3. Embed widget on package README
4. Share custom subdomain in documentation

### For Open Source Contributors
1. Connect GitHub and GitLab
2. Showcase top repositories
3. Link to Dev.to articles
4. Create team for project collaborators

### For Content Creators
1. Connect Dev.to and Medium
2. Display article stats
3. Embed profile on personal website
4. Share subdomain on social media

### For Recruiters
1. Create recruiter profile
2. Search by skills and location
3. View comprehensive developer profiles
4. Contact candidates directly

## 🔧 Troubleshooting

### Platform Sync Failed
- Check username is correct
- Verify API token has required permissions
- Try re-syncing from Profile page
- Check platform API status

### API Key Not Working
- Ensure key hasn't expired
- Check permissions match your needs
- Verify key is included in request header
- Review rate limits

### Widget Not Displaying
- Check script URL is correct
- Verify div ID is "aethex-widget"
- Check browser console for errors
- Ensure username exists

### Domain Transfer Failed
- Verify you own the domain
- Check listing is still active
- Ensure sufficient balance
- Contact support if issue persists

## 📚 Resources

- **Full Documentation**: See NEW_FEATURES.md
- **API Reference**: See API_DOCUMENTATION.md
- **Integration Guide**: See PLATFORM_INTEGRATIONS.md
- **Support**: Open an issue on GitHub

## 🎉 What's Next?

After setting up:
1. ✅ Connect all your platforms
2. ✅ Complete your profile
3. ✅ Add portfolio items
4. ✅ Share your custom subdomain
5. ✅ Embed widget on your site
6. ✅ Create or join teams
7. ✅ Generate API keys for integrations

Happy creating! 🚀
