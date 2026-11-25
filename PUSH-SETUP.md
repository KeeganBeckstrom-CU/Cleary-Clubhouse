# 7 AM morning reminder: one-time setup

Everything here is free (GitHub Actions + Supabase free tier). Do this once, in the repo the players actually use.

## 1. Copy these files into the live app repo
- `sw.js` (next to `index Cleary.html`)
- `push/` folder (`send.mjs`, `package.json`, `push_subs.sql`)
- `.github/workflows/morning-reminder.yml`

## 2. Make the table in Supabase
Supabase > SQL Editor > New query > paste `push/push_subs.sql` > Run.

## 3. Add 4 secrets in GitHub
Repo > Settings > Secrets and variables > Actions > New repository secret:

| Name | Value |
|---|---|
| `SUPABASE_URL` | your project URL (same one in the app) |
| `SUPABASE_SERVICE_KEY` | Supabase > Project Settings > API keys > **service_role / secret** key (never put this in the app) |
| `VAPID_PUBLIC` | `BEuAvnEHuSsSpq3n1m63YwofWfNv_S--aQ57pMRvNRH__56iUYRPzqLIic5uJUt9gsA-_CN_FjLnz1lR2PAf51c` |
| `VAPID_PRIVATE` | the private key Claude gave you in chat (keep it private) |

## 4. Test it
Turn on the reminder from your own phone (Clubhouse > 🔔), then GitHub > Actions > "Morning reminder" > Run workflow. You should get a notification within a few seconds.

## How it works
- Pitchers tap **🔔 Get a 7 AM reminder** in the Clubhouse. iPhone players must open the app from the Home Screen first (Share > Add to Home Screen); the sheet walks them through it. Opening `…/#remind` opens that sheet directly, handy to text the team.
- GitHub runs the job every morning and sends at about 7:00 AM Michigan time, all year (daylight saving handled). It goes out as high urgency and stays on the lock screen until tapped; tapping opens Today's Work.
- iPhone's special "Time Sensitive" level is only available to App Store apps, not web apps. High urgency is the strongest a home-screen web app can send.
- GitHub pauses scheduled jobs after 60 days with no commits to the repo; any commit (or re-enabling it in the Actions tab) turns it back on.
