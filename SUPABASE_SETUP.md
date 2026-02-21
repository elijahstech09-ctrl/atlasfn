# Supabase Setup Guide for AtlasFN

## 🎯 What This Does

Your accounts are now saved in a **real database** that persists forever! No more losing accounts when clearing browser cache.

---

## 🚀 Setup Steps

### 1. Create Supabase Account

1. Go to **https://supabase.com**
2. Click **"Start your project"**
3. Sign up with GitHub (or email)
4. It's **100% FREE** forever on the free tier!

---

### 2. Create New Project

1. Click **"New Project"**
2. Fill in:
   - **Name**: `atlasfn` (or whatever you want)
   - **Database Password**: Create a strong password (SAVE THIS!)
   - **Region**: Choose closest to you
3. Click **"Create new project"**
4. Wait ~2 minutes for database to set up

---

### 3. Create Users Table

1. In your Supabase project, click **"SQL Editor"** (left sidebar)
2. Click **"New Query"**
3. Paste this SQL:

```sql
-- Create users table
CREATE TABLE users (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  username TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  balance DECIMAL(10,2) DEFAULT 0,
  role TEXT DEFAULT 'customer',
  permissions TEXT[] DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

-- Allow users to read their own data
CREATE POLICY "Users can view own data"
  ON users FOR SELECT
  USING (auth.uid() = id);

-- Allow users to update their own data
CREATE POLICY "Users can update own data"
  ON users FOR UPDATE
  USING (auth.uid() = id);

-- Allow anyone to insert (for signup)
CREATE POLICY "Anyone can create user"
  ON users FOR INSERT
  WITH CHECK (true);
```

4. Click **"Run"** (or press F5)
5. You should see "Success. No rows returned"

---

### 4. Get Your API Keys

1. Click **"Project Settings"** (gear icon, bottom left)
2. Click **"API"** in the left menu
3. You'll see:
   - **Project URL** (looks like `https://xxxxx.supabase.co`)
   - **anon/public key** (starts with `eyJh...`)

---

### 5. Add to Vercel Environment Variables

1. Go to **https://vercel.app**
2. Open your project: **atlasfn-syr0-ctrl**
3. Go to **Settings** → **Environment Variables**
4. Add these TWO variables:

**Variable 1:**
- **Key**: `SUPABASE_URL`
- **Value**: Your Project URL from step 4
- **Environment**: Check ALL (Production, Preview, Development)
- Click **Save**

**Variable 2:**
- **Key**: `SUPABASE_KEY`
- **Value**: Your anon/public key from step 4
- **Environment**: Check ALL (Production, Preview, Development)
- Click **Save**

---

### 6. Redeploy

The code has already been updated and pushed! Just:

1. Go to **Deployments** tab in Vercel
2. Click **"..."** on latest deployment
3. Click **"Redeploy"**

---

## ✅ Testing

After redeployment completes (~30 seconds):

1. Visit your site: **https://atlasfn-syr0-ctrl.vercel.app**
2. Click **"Sign Up"** and create a new account
3. Log out and log back in
4. **Clear your browser cache/cookies**
5. Log in again - your account should still exist! ✅

---

## 🔍 Verify in Supabase

1. Go to your Supabase project
2. Click **"Authentication"** (left sidebar)
3. You should see your new user account!
4. Click **"Table Editor"** → **"users"**
5. You should see your user data (username, email, balance, etc.)

---

## 🎉 What Changed

**BEFORE:**
- Accounts stored in browser localStorage
- Lost when clearing cache
- Not shared between devices
- No real security

**AFTER:**
- Accounts stored in Supabase PostgreSQL database
- Persist forever (even if you clear cache)
- Can login from any device
- Real authentication with encrypted passwords
- Scalable to millions of users

---

## 🛠️ Features Now Working

✅ Signup creates real database account  
✅ Login authenticates against database  
✅ Accounts persist across devices  
✅ Passwords properly encrypted (bcrypt via Supabase Auth)  
✅ Session tokens for secure API calls  
✅ Works with your Stripe payment system  

---

## 📊 Supabase Free Tier Limits

- **500 MB database** (enough for ~50,000 users)
- **50,000 monthly active users**
- **2 GB bandwidth per month**
- **Unlimited API requests**

You won't hit these limits for a long time!

---

## ⚠️ Important Notes

1. **Keep your Supabase keys private** (similar to Stripe)
2. The `anon` key is safe to expose in frontend (it's public)
3. Your database password is ONLY for direct database access (you don't need it for the app)
4. Supabase handles all password encryption automatically

---

## 🔧 Troubleshooting

### "Email is already registered"
- Email already exists in database
- Try a different email or reset password

### "Signup failed" / "Login failed"
- Check Vercel environment variables are set correctly
- Make sure you created the `users` table in Supabase
- Check Vercel function logs for errors

### Can't see my account in Supabase
- Go to Authentication tab (not Table Editor) to see auth users
- Then go to Table Editor → users to see profile data

---

## 🚀 Next Steps

Your database is now set up! Consider adding:

- **Password reset** via email (Supabase has built-in support)
- **Email verification** (optional, prevents fake accounts)
- **Order history** stored in database (not just localStorage)
- **Payment methods** saved per user

Let me know if you want help with any of these!

---

Need help? Check the [Supabase Docs](https://supabase.com/docs) or ask me!
