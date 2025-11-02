# Authentication Quick Start

Your authentication system is now fully functional! Here's how to test it:

## How to Use

### 1. Register a New Account
1. Navigate to `/auth` page
2. Click the "Sign Up" tab
3. Fill in:
   - Full Name (e.g., "Alex Developer")
   - Email (e.g., "alex@example.com")
   - Password (minimum 6 characters)
4. Click "Sign Up"
5. You'll be automatically logged in and redirected to `/dashboard`

### 2. Log In
1. Go to `/auth` page
2. Use the "Log In" tab (default)
3. Enter your email and password
4. Click "Log In"
5. You'll be redirected to `/dashboard`

### 3. Edit Your Profile
1. While logged in, navigate to `/profile`
2. Click the "Edit Profile" button
3. Update any of the following:
   - Name
   - Title (e.g., "Full-Stack Developer")
   - Location (e.g., "San Francisco, CA")
   - Avatar URL
   - Bio
4. Click "Save Changes"
5. Your profile will be updated immediately

### 4. Log Out
- Click the "Logout" button in the header (top right)
- You'll be logged out and redirected to the home page

## How It Works

### Data Storage
- **In-Memory Database**: Your app uses an in-memory database for demo purposes
- **Session Management**: Authentication tokens are stored in browser localStorage
- **Persistence**: Data persists during your browser session but resets when the server restarts

### Authentication Flow
1. **Registration**: Creates user with hashed password, generates JWT token
2. **Login**: Validates credentials, returns JWT token
3. **Session**: Token stored in localStorage and sent with each API request
4. **Protected Routes**: Profile page redirects to `/auth` if not logged in

### API Endpoints
- `POST /api/auth/register` - Create new account
- `POST /api/auth/login` - Login to existing account
- `GET /api/auth/me` - Get current user info
- `POST /api/profile/update` - Update profile information

## Moving to Production

To use a real database in production:

1. **Choose a Database**: PostgreSQL, MongoDB, MySQL, etc.
2. **Update `/app/lib/db.server.ts`**: Replace in-memory storage with database client
3. **Environment Variables**: Store database connection strings in `.env`
4. **Update JWT Secret**: Change `JWT_SECRET` in `.env` to a secure random value

### Example with PostgreSQL + Prisma

```bash
# Install Prisma
npm install @prisma/client
npm install -D prisma

# Initialize Prisma
npx prisma init

# Update schema, then migrate
npx prisma migrate dev
```

Then update `db.server.ts` to use Prisma client instead of in-memory Maps.

## Security Notes

- Passwords are hashed with bcrypt (10 rounds)
- JWT tokens expire after 7 days
- Tokens are validated on each protected API call
- In production, always use HTTPS
- Store JWT_SECRET securely

## Next Steps

1. Test registration and login
2. Edit your profile
3. Connect platform accounts (GitHub, Stack Overflow, etc.) via Settings
4. Explore the dashboard and other features

Enjoy your fully functional authentication system!
