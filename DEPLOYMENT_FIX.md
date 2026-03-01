# Fix for 404 Error - Deployment Steps

## Issue
Your frontend is getting a 404 error because the `.env.production` file had a placeholder URL instead of your actual backend URL.

## What I Fixed
✅ Updated `frontend/.env.production` with your actual backend URL: `https://repoai-hmdz.onrender.com/api`

## Next Steps for Vercel Deployment

### 1. Verify Backend is Running
First, test your backend health endpoint:
```bash
curl https://repoai-hmdz.onrender.com/
```

You should see:
```json
{
  "status": "ok",
  "message": "RepoPilot AI Backend is running",
  "endpoints": {
    "analyze": "/api/analyze/:owner/:repo",
    "reviewCode": "/api/review-code"
  }
}
```

### 2. Deploy to Vercel

#### Option A: Using Vercel CLI
```bash
cd frontend
npm install -g vercel
vercel login
vercel --prod
```

#### Option B: Using Vercel Dashboard
1. Go to https://vercel.com/new
2. Import your GitHub repository
3. Set the root directory to `frontend`
4. Add environment variable:
   - Name: `VITE_API_URL`
   - Value: `https://repoai-hmdz.onrender.com/api`
5. Click "Deploy"

### 3. Important: Set Environment Variable in Vercel

Even though `.env.production` has the URL, you MUST also set it in Vercel dashboard:

1. Go to your project in Vercel
2. Click "Settings" → "Environment Variables"
3. Add:
   - **Name**: `VITE_API_URL`
   - **Value**: `https://repoai-hmdz.onrender.com/api`
   - **Environment**: Production
4. Redeploy your app

### 4. Test Your Deployment

After deployment, test with a repository URL like:
```
https://github.com/drawdb-io/drawdb
```

## Troubleshooting

### If you still get 404:
1. Check Render logs to ensure backend is running
2. Verify CORS is enabled in backend (already done)
3. Test backend directly: `curl https://repoai-hmdz.onrender.com/api/analyze/facebook/react`
4. Check Vercel build logs for any environment variable issues

### If backend is sleeping (Render free tier):
- First request may take 30-60 seconds to wake up
- Add a loading state in your app (already implemented)

## Files Updated
- ✅ `frontend/.env.production` - Now has correct backend URL
