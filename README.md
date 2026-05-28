# ViralForge AI - Setup Guide

ViralForge AI is a production-quality SaaS for generating viral TikTok/Shorts content using Next.js, OpenAI, Supabase, and Stripe.

## 1. Local Setup

### Prerequisites
- Node.js 18+
- npm/pnpm/yarn
- A Supabase Project
- An OpenAI API Key
- A Stripe Account

### Installation
```bash
cd viralforge-ai
npm install
```

### Environment Variables
Create a `.env.local` file with the following:
```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

OPENAI_API_KEY=your_openai_api_key

STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key

NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

## 2. Supabase Setup

### Database Schema
Run the SQL found in `supabase/migrations/20240526_initial_schema.sql` in the Supabase SQL Editor. This will create:
- `profiles` table (linked to auth.users)
- `generations` table (storing AI content)
- Auth Trigger (automatically creates profiles on signup)
- RLS Policies (secures user data)

### Authentication
Enable **Email/Password** provider in the Supabase Auth settings.

## 3. Stripe Setup

### Products & Prices
1. Create a "Premium" product in Stripe.
2. Add a recurring price (e.g., $19/mo).
3. Copy the **Price ID** for your checkout integration.

### Webhooks
1. Install the [Stripe CLI](https://stripe.com/docs/stripe-cli).
2. Run `stripe listen --forward-to localhost:3000/api/webhooks/stripe`.
3. Copy the provided webhook secret to your `.env.local`.

## 4. Deployment

### Vercel
1. Push your code to GitHub.
2. Import the project to Vercel.
3. Add all environment variables.
4. Set the `NEXT_PUBLIC_SITE_URL` to your production domain.
5. Update your Stripe Webhook URL to `https://your-domain.com/api/webhooks/stripe`.

## 5. Development
```bash
npm run dev
```
Visit `http://localhost:3000` to see your app.
