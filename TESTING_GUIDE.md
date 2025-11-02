# Testing Guide

This guide walks you through testing all features of the DevHub Aggregator platform.

## Prerequisites

Before testing, ensure you have:
- Environment variables configured (see ENV_SETUP.md)
- Development server running: `npm run dev`
- Valid GitHub username for testing
- Valid Stack Overflow user ID (optional)

## 1. User Registration & Authentication

### Register a New User

1. Navigate to http://localhost:5173
2. Click "Get Started" or navigate to `/auth`
3. Fill in the registration form:
   - Email: test@example.com
   - Password: TestPassword123!
   - Name: Test User
   - Username: testuser
4. Click "Create Account"

**Expected Result:**
- User should be created successfully
- Redirected to `/dashboard`
- Token stored in localStorage
- User info displayed in header

### Test Login

1. Log out (if logged in)
2. Navigate to `/auth`
3. Click "Sign In" tab
4. Enter credentials:
   - Email: test@example.com
   - Password: TestPassword123!
5. Click "Sign In"

**Expected Result:**
- Successfully logged in
- Redirected to `/dashboard`
- Dashboard shows empty state (no connections yet)

## 2. GitHub Integration

### Sync GitHub Profile

1. From dashboard, click "Sync Profiles" or navigate to `/profile`
2. In the "GitHub" section:
   - Enter a valid GitHub username (e.g., "octocat" or your own)
   - Click "Connect GitHub"
3. Wait for sync to complete

**Expected Result:**
- Loading state shown during sync
- Profile data appears:
  - Avatar, name, bio, location
  - Public repos count
  - Followers/following count
  - Account creation date
- Top repositories displayed with:
  - Repository name
  - Description
  - Language
  - Stars/forks count
- Recent activity shown:
  - Commits
  - Pull requests
  - Issues
  - Code reviews

### Verify GitHub Data Quality

Check that:
- Avatar image loads correctly
- Repository languages are accurate
- Activity dates are recent
- Links to GitHub profile/repos work

## 3. Stack Overflow Integration

### Sync Stack Overflow Profile

1. Navigate to `/profile`
2. In the "Stack Overflow" section:
   - Enter a Stack Overflow user ID (e.g., "1" for Jon Skeet)
   - Click "Connect Stack Overflow"
3. Wait for sync to complete

**Expected Result:**
- Profile data appears:
  - Reputation score
  - Badge counts (gold, silver, bronze)
  - Profile image
  - Location
- Top answers/questions displayed
- Tag expertise shown
- Links to Stack Overflow profile work

### Test Without API Key

If you don't have a Stack Overflow API key:
- The integration will still work
- Rate limits will be lower (300 requests/day)
- Some features may be slower

## 4. Aggregated Profile View

### View Combined Profile

1. After syncing both GitHub and Stack Overflow, navigate to `/profile`
2. Review the aggregated data:
   - Profile header shows latest avatar
   - Bio from GitHub displayed
   - Both platforms listed under "Connected Platforms"

**Expected Result:**
- Unified profile view
- All connections shown with status
- Links to original profiles work
- Last sync timestamp displayed

### Test Skill Aggregation

1. Check the "Skills" section
2. Verify skills from both platforms:
   - GitHub: Languages from repositories
   - Stack Overflow: Tags from answers/questions

**Expected Result:**
- Combined skill scores
- Skills sorted by proficiency
- Visual indicators (progress bars/badges)

## 5. Analytics Dashboard

### View Analytics

1. Navigate to `/dashboard`
2. Review the analytics sections:
   - Overview metrics (total connections, profile views, etc.)
   - Activity timeline
   - Skill distribution chart
   - Platform breakdown

**Expected Result:**
- Charts render correctly
- Data reflects your synced profiles
- Filters work (time range, platform)
- Hover states show details

### Test Analytics Export

1. Click "Export Analytics" button
2. Choose format (CSV or JSON)
3. Download file

**Expected Result:**
- File downloads successfully
- Contains accurate data
- Properly formatted

## 6. Profile Syncing

### Manual Sync

1. Navigate to `/profile`
2. Click "Sync Now" button in settings dialog
3. Wait for sync to complete

**Expected Result:**
- Loading indicator shown
- All connected platforms re-synced
- Updated data reflected immediately
- Success message shown

### Test Sync Error Handling

1. Enter an invalid GitHub username (e.g., "this-user-does-not-exist-123456")
2. Attempt to connect

**Expected Result:**
- Error message displayed
- User not redirected
- Previous data (if any) remains intact
- Can retry with correct username

## 7. Settings & Configuration

### Update Settings

1. Open settings dialog (gear icon in header)
2. Test each setting:
   - Enable/disable analytics tracking
   - Set profile visibility (public/private)
   - Configure auto-sync preferences
   - Update notification settings

**Expected Result:**
- Settings save successfully
- Changes reflected immediately
- Persisted after page reload

### Profile Privacy

1. Set profile to "Private" in settings
2. Try accessing profile from another browser (not logged in)

**Expected Result:**
- Private profiles not accessible without auth
- Public profiles visible to all
- Profile links respect privacy settings

## 8. Responsive Design

### Test Mobile View

1. Open browser DevTools
2. Switch to mobile viewport (375x667)
3. Navigate through all pages

**Expected Result:**
- All pages responsive
- Navigation works on mobile
- Charts resize appropriately
- Touch interactions work
- No horizontal scrolling

### Test Tablet View

1. Switch to tablet viewport (768x1024)
2. Navigate through pages

**Expected Result:**
- Layout adapts to tablet size
- Sidebar behavior appropriate
- Cards reflow correctly

## 9. Performance Testing

### Test Load Times

1. Open Network tab in DevTools
2. Navigate to different pages
3. Check load times

**Expected Result:**
- Initial load < 3 seconds
- Navigation < 1 second
- API calls < 2 seconds
- No memory leaks

### Test with Multiple Connections

1. Sync multiple platforms
2. Navigate to dashboard
3. Check performance

**Expected Result:**
- Dashboard loads smoothly
- Charts render without lag
- Scrolling is smooth
- No freezing

## 10. Error Scenarios

### Network Errors

1. Disconnect internet (or use DevTools to simulate offline)
2. Try syncing profiles

**Expected Result:**
- Clear error message
- Retry option available
- App doesn't crash
- Previous data still visible

### Invalid Tokens

1. Manually corrupt JWT token in localStorage
2. Refresh page

**Expected Result:**
- Redirected to login
- Token cleared
- No console errors

### API Rate Limits

If using GitHub without token:
1. Make multiple sync requests quickly

**Expected Result:**
- Rate limit error shown
- Helpful message about adding token
- Can retry after waiting

## Automated Testing Checklist

- [ ] User registration works
- [ ] User login works
- [ ] GitHub sync completes
- [ ] Stack Overflow sync completes
- [ ] Profile data displays correctly
- [ ] Analytics charts render
- [ ] Settings save properly
- [ ] Responsive on mobile
- [ ] Responsive on tablet
- [ ] Error handling works
- [ ] Export functionality works
- [ ] Links open correctly
- [ ] Images load properly
- [ ] Performance acceptable

## Known Issues

Document any issues found during testing:

1. **Issue:** [Description]
   - **Reproduce:** [Steps]
   - **Expected:** [What should happen]
   - **Actual:** [What actually happens]
   - **Severity:** High/Medium/Low

## Next Steps

After completing all tests:
1. Document any bugs found
2. Test performance optimizations
3. Prepare for production deployment
4. Set up monitoring and analytics
