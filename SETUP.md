# Dev Daily Tracker: setup (about 30 minutes)

The page is a static site on GitHub Pages. The data lives in a free Supabase database.
Each developer signs in with their email and can only ever read their own tasks and updates. The manager (you) sees everyone.

Files in this folder:
- `index.html` is the whole app.
- `config.js` holds your two Supabase values.
- `supabase/schema.sql` creates the tables and the privacy rules.

---

## Step 1. Create the Supabase project (5 min)
1. Go to https://supabase.com and sign in (GitHub login is fine).
2. **New project**. Name it `dev-tracker`. Choose a strong database password and save it somewhere. Pick the region closest to you (for India: Mumbai, `ap-south-1`).
3. Wait about 2 minutes while it sets up.

## Step 2. Create the tables and privacy rules (5 min)
1. Open `supabase/schema.sql` in a text editor.
2. Near the top, replace `REPLACE_WITH_YOUR_MANAGER_EMAIL@gmail.com` with the Gmail you will sign in with as manager. This email is the only one that sees everyone's data.
3. Check the other emails (Arsh, Madhav, and the test account). Only emails on this list can use the tracker.
4. In Supabase, open **SQL Editor > New query**, paste the whole file and click **Run**. It should say "Success".

To add or remove a person later, edit the list with another query, for example:
```sql
insert into public.allowed_users (email, name, role) values ('new.person@gmail.com', 'New Person', 'developer');
```

## Step 3. Copy your keys (2 min)
1. In Supabase open **Project Settings > API**.
2. Copy the **Project URL** and the **anon public** key.
3. Paste both into `config.js`.
4. The anon key is meant to be public, because the privacy rules protect the data. Never paste the `service_role` key anywhere.

## Step 4. Put the page on GitHub Pages (10 min)
1. On GitHub create a new **public** repository, for example `dev-tracker`. (GitHub Pages is free only for public repos. The page holds no secrets.)
2. Upload `index.html` and `config.js` to the repo's main branch. You do not need to upload `SETUP.md` or `supabase/`.
3. In the repo go to **Settings > Pages**. Under "Build and deployment" choose **Deploy from a branch**, branch `main`, folder `/ (root)`, then **Save**.
4. After about a minute the site is live at `https://YOUR-GITHUB-NAME.github.io/dev-tracker/`.

## Step 5. Tell Supabase the site address (2 min)
In Supabase open **Authentication > URL Configuration**:
- **Site URL:** `https://YOUR-GITHUB-NAME.github.io/dev-tracker/`
- **Redirect URLs:** add the same address.

Without this, the sign-in link in the email sends people to the wrong place.

## Step 6. Sign in and test (5 min)
1. Open the site and type your manager email, then **Email me a sign-in link**. Open the link from your inbox in the same browser.
2. You should land in the manager view with a **View sample data** button. Use it to see how everything works.
3. Open the site in a private window and sign in as the test developer account from your invite list. Post a daily update and add a task.
4. Back in your manager window, reload. The update should appear under **Today**.
5. When it all works, ask Arsh and Madhav to open the link and sign in.

---

## Optional: "Continue with Google" button
The email link works with no extra setup. To make the Google button work too:
1. In Google Cloud Console (https://console.cloud.google.com) create a project, then **APIs & Services > OAuth consent screen** (External, add your email) and **Credentials > Create credentials > OAuth client ID** (type: Web application).
2. Add this under **Authorized redirect URIs**: `https://YOUR-PROJECT-REF.supabase.co/auth/v1/callback` (copy the exact address from Supabase **Authentication > Providers > Google**).
3. Paste the Client ID and Client secret into Supabase **Authentication > Providers > Google** and enable it.
4. While the consent screen is in "Testing", add Arsh, Madhav and the test email as test users.

## Good to know
- **Email limits:** Supabase's built-in email sender is limited to a few emails per hour. For 3 or 4 people this is usually enough. If sign-in emails stop arriving, set up your own sender under **Project Settings > Authentication > SMTP** (Brevo and Resend both have free plans), or use the Google button.
- **Free projects pause** after a week with no activity. Daily use keeps it awake. If it pauses, click "Restore" in the Supabase dashboard.
- **Backups:** on the free plan, export now and then from **Table Editor > Export to CSV** for `tasks` and `daily_logs`.
- **Privacy:** developers can only read and write their own rows. The rules are enforced inside the database, not just in the page.
