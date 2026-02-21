# Stripe Setup Guide for AtlasFN

## ✅ What's Already Done

1. ✅ **Publishable Key** - Added to your HTML (pk_live_...)
2. ✅ **Serverless Function** - Created at `api/create-payment-intent.js`
3. ✅ **Package.json** - Created with Stripe dependency
4. ✅ **HTML Updated** - Now calls secure serverless function

---

## 🔐 Final Step: Add Secret Key to Vercel

**DO THIS NOW to enable real payments:**

### 1. Go to Vercel Dashboard
https://vercel.app

### 2. Open Your Project
Click: **atlasfn-syr0-ctrl**

### 3. Go to Settings
Click: **Settings** tab → **Environment Variables**

### 4. Add Secret Key
- **Key**: `STRIPE_SECRET_KEY`
- **Value**: `YOUR_STRIPE_SECRET_KEY` (the one starting with `sk_live_...` that you already added to Vercel)
- **Environment**: Select **ALL** (Production, Preview, Development)
- Click: **Save**

### 5. Redeploy (Optional but Recommended)
Go to **Deployments** tab → Click **...** on latest → **Redeploy**

---

## 🚀 How It Works Now

```
Customer enters card
     ↓
Your HTML (Browser)
     ↓ [Calls /api/create-payment-intent]
     ↓
Vercel Serverless Function
     ↓ [Uses STRIPE_SECRET_KEY from environment]
     ↓
Stripe API
     ↓ [Returns client_secret]
     ↓
Back to Browser
     ↓
Stripe.js confirms payment
     ↓
✅ Payment Complete!
```

**Secret key never touches the browser! ✅**

---

## 📋 Next Steps

After adding the environment variable to Vercel:

1. **Deploy your changes:**
   ```bash
   git add .
   git commit -m "Add Stripe serverless function"
   git push
   ```

2. **Wait for Vercel to deploy** (~30 seconds)

3. **Test a payment:**
   - Use Stripe test card: `4242 4242 4242 4242`
   - Expiry: Any future date
   - CVV: Any 3 digits

4. **Check Stripe Dashboard:**
   https://dashboard.stripe.com/payments
   - You should see the payment!

---

## 🔥 IMPORTANT Security Note

**NEVER share your secret key again!** The one you gave me should be:
1. ❌ Deleted/rolled in Stripe Dashboard
2. ✅ Replaced with a new secret key
3. ✅ New key added to Vercel environment variables only

---

## ⚠️ Troubleshooting

### "Payment intent creation failed"
- Check Vercel environment variable is saved
- Make sure you redeployed after adding the variable
- Check Vercel function logs: Dashboard → Deployments → Functions

### Payments not showing in Stripe
- Make sure you're in **Live Mode** not Test Mode in Stripe Dashboard
- Check the API key starts with `sk_live_` not `sk_test_`

### Function timeout
- Vercel free tier has 10-second timeout
- Payment intents creation is fast (<1 second normally)

---

## ✅ Success Checklist

- [ ] Environment variable `STRIPE_SECRET_KEY` added to Vercel
- [ ] Code pushed to GitHub
- [ ] Vercel redeployed successfully
- [ ] Test payment completed
- [ ] Payment shows in Stripe Dashboard
- [ ] Old secret key rolled/deleted in Stripe

---

Need help? Check:
- Vercel Logs: https://vercel.com/docs/concepts/functions/serverless-functions
- Stripe Docs: https://stripe.com/docs/payments/payment-intents
