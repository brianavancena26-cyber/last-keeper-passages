# The Last Keeper of the Passages — individual tour leaderboard

This package contains a GitHub Pages mobile game (`index.html`) and a Cloudflare Worker + KV backend (`worker/`). Guests do not create accounts. Each guest joins with a nickname and a shared **tour code** (e.g. `PARIS-1111`) and receives a random private session token in their browser. Only players with the same tour code appear on each other's leaderboard.

## Setup (organizer only)
1. Create a free Cloudflare account. In Cloudflare, create a Workers KV namespace, e.g. `LAST_KEEPER_GAME`. Copy its namespace ID.
2. Edit `worker/wrangler.toml` and replace `REPLACE_WITH_YOUR_KV_NAMESPACE_ID` with that ID.
3. Install Node.js, then from the `worker` folder run `npx wrangler login` and `npx wrangler deploy`. This deploys the Worker and prints its `https://...workers.dev` URL. Wrangler may ask you to create a workers.dev subdomain.
4. Edit `index.html`: replace `PASTE_YOUR_CLOUDFLARE_WORKER_URL_HERE` with the Worker URL (include `https://`).
5. Upload `index.html` to a public GitHub repository. In Settings > Pages, deploy from `main` and `/ (root)`.
6. Give all guests on one scheduled tour the same code; use a **different, unpredictable code for each scheduled departure**. Share the public GitHub Pages game URL or its QR code.

## Game rules
- Each correct chapter: +100 points; each revealed hint: -25 points, with a minimum score of 0.
- The Worker validates answers and computes points; guests cannot simply edit their browser score.
- Individual scores and stage are saved in KV; the browser stores a private session ID/token to resume on that device.
- The top 30 scores for the entered tour code are displayed, refreshed every 20 seconds. Ties sort by chapter reached then nickname.
- No guest login or app download. Guests enter nicknames only; avoid full names.

## Important operational limitations
- **This is a starter deployment, not a hardened commercial platform.** Anyone with the tour code can join the tour leaderboard; there is no organizer-only code issuance, code expiration, guide moderation, or anti-spam protection. Share codes only with participants. Rejoining creates another entry; browser data deletion loses the existing session. The code is not a secret once shared.
- Worker/KV has eventual consistency: cross-device leaderboard updates may take time, and Cloudflare usage limits apply. For large groups or higher assurance, use a database with transactional updates and an organizer dashboard.
- There is no server-enforced one-attempt limit or GPS verification. Answers can be guessed, and the game is not a secure competition.
- The current backend lists all player records to find tour members, so it is suitable only for **small pilots**. For production, implement per-tour indexed records, expiration/retention policy, rate limits, and consent/privacy notices.
- The Worker currently permits cross-origin requests from any site. Restrict CORS to your published domain before public launch.
- The architectural-plan and mosaic puzzles are still simplified text-based versions. Route-test all clues and historical details before selling.
- Cloudflare is an independent service, not hosted by GitHub Pages. No backend URL or deployment is configured in this package.

## Files
- `index.html`: guest-facing GitHub Pages game
- `worker/index.js`: Cloudflare Worker API and server-side scoring
- `worker/wrangler.toml`: deployment configuration

## Guide dashboard and welcome registration (v3)
- The guest welcome screen now explains the story, scoring, privacy, and registration. A guest registers by entering a nickname and the guide's scheduled-tour code.
- Upload `guide-dashboard.html` to the same repository root as `index.html`; its public URL ends in `/guide-dashboard.html`. Do not share the guide dashboard link or guide key with guests.
- In Cloudflare Worker settings, add a secret environment variable named `GUIDE_KEY` with a long random value known only to guides; redeploy the Worker if needed. Never place this secret in GitHub files.
- Replace `PASTE_YOUR_CLOUDFLARE_WORKER_URL_HERE` in **both** HTML files with your deployed Worker URL. Deploy the updated `worker/index.js` before using the guide dashboard.
- Open the guide dashboard, enter the tour code and secret, and press Refresh roster. The guide sees each guest's nickname, stage, points, hints and completion. The guide key is sent to the Worker for verification, not stored by this page.
- This is a pilot: the public registration is not verified against bookings; the dashboard URL is public but data requires the guide secret. For production, add consent/retention policy, stronger guide authentication, abuse controls, indexed storage and booking verification.
