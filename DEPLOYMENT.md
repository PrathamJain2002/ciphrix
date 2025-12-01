# Deployment Guide: Vercel (Frontend) + Render (Backend)

This guide will walk you through deploying the MERN Task Management App to Vercel (frontend) and Render (backend).

## Prerequisites

1. GitHub account
2. Vercel account (sign up at https://vercel.com)
3. Render account (sign up at https://render.com)
4. MongoDB Atlas account (free tier available at https://www.mongodb.com/cloud/atlas)

---

## Step 1: Set Up MongoDB Atlas

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account or sign in
3. Create a new cluster (choose FREE tier)
4. Create a database user:
   - Go to **Database Access** → **Add New Database User**
   - Choose **Password** authentication
   - Save the username and password securely
5. Whitelist IP addresses:
   - Go to **Network Access** → **Add IP Address**
   - Click **Allow Access from Anywhere** (0.0.0.0/0) for development
6. Get your connection string:
   - Go to **Clusters** → Click **Connect** on your cluster
   - Choose **Connect your application**
   - Copy the connection string (it looks like: `mongodb+srv://username:password@cluster.mongodb.net/`)
   - Replace `<password>` with your database user password
   - Add database name at the end: `mongodb+srv://username:password@cluster.mongodb.net/taskmanagement`

---

## Step 2: Deploy Backend to Render

### 2.1 Push Code to GitHub

1. Make sure your code is pushed to GitHub:
   ```bash
   git add .
   git commit -m "Prepare for deployment"
   git push origin main
   ```

### 2.2 Create Web Service on Render

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click **New +** → **Web Service**
3. Connect your GitHub repository
4. Select the repository: `ciphrix`
5. Configure the service:
   - **Name**: `task-management-api` (or any name you prefer)
   - **Region**: Choose closest to you
   - **Branch**: `main`
   - **Root Directory**: `backend`
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
6. Click **Advanced** and add Environment Variables:
   ```
   NODE_ENV = production
   PORT = 10000
   MONGODB_URI = mongodb+srv://username:password@cluster.mongodb.net/taskmanagement
   JWT_SECRET = your_super_secret_jwt_key_change_this_in_production
   FRONTEND_URL = https://your-vercel-app.vercel.app
   ```
   **Note**: You'll update `FRONTEND_URL` after deploying the frontend.

7. Click **Create Web Service**
8. Wait for deployment to complete (takes 2-3 minutes)
9. Copy your backend URL (e.g., `https://task-management-api.onrender.com`)

---

## Step 3: Deploy Frontend to Vercel

### 3.1 Update Backend CORS (if needed)

After getting your Vercel URL, update the `FRONTEND_URL` environment variable in Render:
1. Go to Render dashboard → Your service → **Environment**
2. Update `FRONTEND_URL` to your Vercel URL

### 3.2 Deploy to Vercel

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click **Add New...** → **Project**
3. Import your GitHub repository: `ciphrix`
4. Configure the project:
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`
5. Add Environment Variables:
   ```
   VITE_API_URL = https://your-render-backend-url.onrender.com/api
   ```
   Replace `your-render-backend-url` with your actual Render backend URL.

6. Click **Deploy**
7. Wait for deployment (takes 1-2 minutes)
8. Copy your Vercel URL (e.g., `https://ciphrix.vercel.app`)

### 3.3 Update Backend CORS

1. Go back to Render dashboard
2. Update `FRONTEND_URL` environment variable to your Vercel URL
3. Render will automatically redeploy

---

## Step 4: Verify Deployment

1. **Test Backend**:
   - Visit: `https://your-backend-url.onrender.com/api/health`
   - Should return: `{"message":"Server is running"}`

2. **Test Frontend**:
   - Visit your Vercel URL
   - Try signing up a new user
   - Create a task
   - Test all CRUD operations

---

## Step 5: Seed Admin User (Optional)

To create an admin user, you can:

1. **Option 1: Use Render Shell**
   - Go to Render dashboard → Your service → **Shell**
   - Run: `npm run seed`

2. **Option 2: Create via API**
   - Use Postman or curl to call the signup endpoint
   - Then manually update the user role in MongoDB Atlas

3. **Option 3: Create via MongoDB Atlas**
   - Go to MongoDB Atlas → Browse Collections
   - Find the `users` collection
   - Insert a document with role: "admin"

---

## Environment Variables Summary

### Render (Backend)
```
NODE_ENV=production
PORT=10000
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/taskmanagement
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
FRONTEND_URL=https://your-vercel-app.vercel.app
```

### Vercel (Frontend)
```
VITE_API_URL=https://your-render-backend-url.onrender.com/api
```

---

## Troubleshooting

### Backend Issues

1. **MongoDB Connection Error**
   - Verify MongoDB Atlas connection string
   - Check IP whitelist in MongoDB Atlas
   - Ensure password is URL-encoded if it contains special characters

2. **CORS Errors**
   - Verify `FRONTEND_URL` in Render matches your Vercel URL exactly
   - Check that URL doesn't have trailing slash

3. **Build Fails**
   - Check Render logs for specific errors
   - Ensure `package.json` has correct start script

### Frontend Issues

1. **API Calls Fail**
   - Verify `VITE_API_URL` in Vercel environment variables
   - Check browser console for CORS errors
   - Ensure backend is running and accessible

2. **Build Fails**
   - Check Vercel build logs
   - Ensure all dependencies are in `package.json`

3. **404 on Refresh**
   - Vercel.json should handle this with rewrites (already configured)

---

## Updating Deployed Code

1. Make changes locally
2. Commit and push to GitHub:
   ```bash
   git add .
   git commit -m "Your commit message"
   git push origin main
   ```
3. Both Vercel and Render will automatically redeploy

---

## Free Tier Limitations

### Render
- Services spin down after 15 minutes of inactivity
- First request after spin-down takes ~30 seconds
- 750 hours/month free

### Vercel
- Unlimited deployments
- Automatic HTTPS
- Global CDN

### MongoDB Atlas
- 512 MB storage
- Shared cluster

---

## Production Recommendations

1. **Use Strong JWT Secret**: Generate a random string for production
2. **Enable MongoDB Atlas Backups**: Upgrade to paid tier if needed
3. **Add Error Monitoring**: Consider Sentry or similar
4. **Set Up Custom Domains**: Configure in both Vercel and Render
5. **Enable Rate Limiting**: Add rate limiting middleware to backend

---

## Support

If you encounter issues:
1. Check deployment logs in both Vercel and Render
2. Verify all environment variables are set correctly
3. Test API endpoints directly using Postman or curl
4. Check browser console for frontend errors

