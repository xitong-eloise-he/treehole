# Treehole

An anonymous community for sharing what weighs on you and listening to others. Say the things you can't tell people who know you.

**Live site:** https://eloisehe.here/treehole/

## Features

- **Sign-in required**: email code or magic link. Others see only a random nickname (like `tree#0382`), never your email.
- **Post anonymously**: write a secret, reflection or note and tag it with a topic.
- **Square**: browse everyone's posts, filter by type, and comment to comfort and talk with each other.
- **Topics**: browse by feeling (sad, anxious, angry, lonely, exhausted, happy) or by area of life (school, work, love, family, friends, growing up, wellbeing, other).
- **Private messages**: start a one-to-one anonymous chat from any post or comment. Any message can be reported, and either person can block the chat.
- **Anonymous ratings and reports**: mark posts and comments helpful or not helpful with an optional one-line note; report harassment, hate, spam or self-harm risk.
- **Admin panel**: admins review reports, hide content and ban authors. They can read only the private messages that were reported.
- **Flood control**: each person can send at most 5 posts, 20 comments and 60 messages every 10 minutes.
- Chinese/English switch and crisis hotlines on every page.

## Stack

- Static front end on GitHub Pages: `index.html`, `style.css`, `app.js`, `i18n.js`, `config.js`.
- Database, login and permissions on Supabase: `supabase/schema.sql`, `supabase/002_communities.sql`.
- Login emails sent through Brevo SMTP.

## Setup

1. **Supabase**: create a project at https://supabase.com. In **SQL Editor**, run `supabase/schema.sql`, then `supabase/002_communities.sql`.
2. **Config**: copy the Project URL and the publishable (anon) key from **Project Settings → API Keys** into `config.js`. That key is meant to be public; the database rules decide what it can do. Never put the secret or `service_role` key in this repo.
3. **Auth URLs**: under **Authentication → URL Configuration**, set Site URL and Redirect URLs to your GitHub Pages address.
4. **Email**: under **Authentication → Emails → SMTP Settings**, add custom SMTP (for example Brevo: host `smtp-relay.brevo.com`, port `587`). Then edit the **Magic Link** template to include `{{ .Token }}` so emails carry a sign-in code. To use link-only emails instead, set `emailCode: false` in `config.js`.
5. **Admin**: sign in once, then run:
   ```sql
   update public.profiles set is_admin = true
     where id = (select id from auth.users where email = 'YOUR_EMAIL');
   ```
6. **GitHub Pages**: under **Settings → Pages**, deploy from branch `main`, folder `/ (root)`.

When you change `style.css` or any `.js` file, bump the `?v=` numbers in `index.html` so browsers fetch the new files.

## Notes

- **Mainland China**: without a VPN, GitHub Pages and Supabase are unreliable there. Serving users in mainland China needs a mainland host, an ICP filing and real-name (phone) verification.
- **Moderation**: anonymous emotional-support spaces attract abuse. Check the reports panel often.
- **Hotlines**: check the numbers in `index.html` and `i18n.js` before launch, and add lines for wherever your users are.
