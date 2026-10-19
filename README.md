# Dayflow — Cloud-first Day Scheduler

A vanilla HTML/CSS/JavaScript starter for a personal day scheduler. Supabase PostgreSQL is the source of truth; the app does not persist app data to localStorage.

## Included in this starter

- Supabase email/password sign-in and signup
- Cloud-backed tasks and task status tracking
- Weekly timetable blocks, routine templates and drag-and-drop day changes
- Planned vs actual task timestamps (basic foundation)
- Study/coding tracker
- Habit check-ins
- Focus timer with cloud session history
- Basic analytics dashboard
- Secure AI Edge Function scaffold (provider integration is not yet implemented)
- RLS policies and schema migration
- Vercel static deployment configuration

## 1. Create Supabase project

1. Create a project at https://supabase.com/dashboard.
2. Open **SQL Editor** and run `supabase/schema.sql`.
3. Open **Project Settings → API** and copy the project URL and publishable key (or legacy anon key).
4. In `app.js`, replace:
   - `https://YOUR_PROJECT.supabase.co`
   - `YOUR_SUPABASE_PUBLISHABLE_KEY`
5. In Supabase **Authentication → URL Configuration**, set your local development URL and later your production Vercel URL. Configure email confirmation as desired.

Never put a Supabase service-role key or AI provider secret in `app.js`.

## 2. Run locally for development

Because the app uses Supabase from the browser, serve the folder over HTTP rather than opening `index.html` as a file. For example:

```bash
python -m http.server 8000
```

Then open http://localhost:8000. `localhost` here is only the development web server; saved application data goes to Supabase after login. Do not use localStorage as the database.

## 3. Push to GitHub

Create an empty GitHub repository, then from this folder:

```bash
git init
git add .
git commit -m "Initial Dayflow cloud scheduler"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

## 4. Deploy to Vercel

1. Import the GitHub repository in https://vercel.com.
2. Framework preset: **Other**; build command: none; output directory: `.`.
3. Deploy.
4. Add your production domain to Supabase Auth's Site URL and Redirect URLs.
5. Test signup, sign-in, task creation, timetable persistence, and cross-device sync.

This starter's vanilla frontend configuration is currently set in `app.js`, so a public Supabase URL and publishable key are visible in the client by design. Security must rely on RLS. For a larger production app, inject public config at build time.

## 5. AI planner

The browser calls `supabase.functions.invoke("ai-planner")`. The included Edge Function is an authenticated scaffold and returns a not-configured response until an AI provider is integrated.

Deploy with Supabase CLI (after installing and logging in):

```bash
supabase functions deploy ai-planner
supabase secrets set AI_API_KEY=YOUR_PROVIDER_SECRET
```

Then implement the provider call in `supabase/functions/ai-planner/index.ts`. Do not expose `AI_API_KEY` in browser code or GitHub. Validate all model output before allowing a user to approve/apply a plan.

## Important limitations before production

This is a runnable starter, not a finished commercial product. Specifically:

- Push notifications and scheduled server-side reminder delivery need to be implemented.
- Timetable drag-and-drop currently changes a block's weekday; it does not yet move exact times.
- The current timer UI runs in the active page and is not a background timer when the browser is closed.
- Add stricter parent-child ownership checks and overlap constraints for timetable rows.
- Add pagination, rate limits, account deletion/export, error reporting, accessibility testing, and two-user RLS tests.
- Add proper planned duration editing and richer actual-time tracking.
- Analytics are starter-level; add date-scoped aggregation for accurate weekly reports.

## Data and privacy

- Supabase PostgreSQL is the persistence layer.
- RLS restricts table rows by authenticated user.
- `localStorage` is not used as the app's data store.
- The Supabase publishable/anon key is public; never ship service-role keys or AI secrets to the browser.
