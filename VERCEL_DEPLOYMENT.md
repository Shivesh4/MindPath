# Vercel Deployment Guide

## Changes Made for Vercel Deployment

### 1. Updated `package.json`
- Added `postinstall` script to generate Prisma client automatically after npm install
- Updated `build` script to include `prisma generate` before building

### 2. Created `vercel.json`
- Configured Vercel build settings
- Set framework to Next.js
- Configured region settings

### 3. Updated `next.config.js`
- Fixed webpack configuration to only externalize canvas on server-side
- Maintained existing image remote patterns
- Kept TypeScript and ESLint build error ignoring (for now)

### 4. Created `.vercelignore`
- Excludes test files, development files, and environment files from deployment

## Required Environment Variables in Vercel

Set these in your Vercel project settings (Settings → Environment Variables):

### Required Variables:
- `DATABASE_URL` - PostgreSQL connection string (e.g., from Supabase)
- `JWT_SECRET` - Secret string for JWT token signing (use a long, random string)
- `JWT_EXPIRES_IN` - JWT expiration time (e.g., "24h")
- `BASE_URL` - Your production URL (e.g., "https://your-app.vercel.app")
- `GROQ_API_KEY` - API key for Groq AI (from console.groq.com)

### Optional Variables:
- `EMAIL_OUTGOING_SERVER` - SMTP server hostname (defaults to 'mail.mxs7500.uta.cloud')
- `EMAIL_SMTP_PORT` - SMTP port (defaults to 465)
- `EMAIL_USERNAME` - Email username for sending emails
- `EMAIL_PASSWORD` - Email password for sending emails
- `NODE_ENV` - Set to "production" (Vercel sets this automatically)

## Build Process

The build process will:
1. Run `npm install` (which triggers `postinstall` → `prisma generate`)
2. Run `npm run build` (which runs `prisma generate && next build`)

## Prisma Setup

Make sure your Prisma schema is up to date. Vercel will:
1. Install dependencies
2. Generate Prisma client (via postinstall script)
3. Build the Next.js application

## Deployment Steps

1. **Push your code to GitHub/GitLab/Bitbucket**
2. **Import project in Vercel**
   - Go to vercel.com
   - Click "New Project"
   - Import your repository
3. **Set Environment Variables**
   - Add all required environment variables listed above
4. **Deploy**
   - Vercel will automatically detect Next.js and use the correct build settings
   - The build will run automatically

## Troubleshooting

### Build Fails with Prisma Errors
- Ensure `DATABASE_URL` is set correctly in Vercel
- Check that Prisma migrations are committed to your repository
- Verify `prisma/schema.prisma` is correct

### Build Fails with Module Not Found
- Ensure all dependencies are in `package.json`
- Check that `node_modules` is not committed (should be in `.gitignore`)

### Runtime Errors
- Check Vercel function logs in the dashboard
- Verify all environment variables are set
- Check that database is accessible from Vercel's IP ranges (if using IP restrictions)

## Notes

- The `ws-server.js` file is excluded from deployment (WebSocket server for development only)
- Test files are excluded from deployment
- Environment files (`.env*`) should NOT be committed - set variables in Vercel dashboard instead

