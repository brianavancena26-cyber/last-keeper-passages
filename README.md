# The Last Keeper of the Passages

Mobile-first, no-login scavenger hunt for Paris Top Sights Tours.

## Publish free with GitHub Pages
1. Create a **public** GitHub repository, e.g. `last-keeper-passages`.
2. Upload `index.html` to the repository root and commit.
3. In **Settings → Pages**, choose **Deploy from a branch**, `main`, `/ (root)` and Save.
4. Wait for GitHub Pages to publish. Your public game address will generally be `https://YOUR-USERNAME.github.io/last-keeper-passages/`.
5. Share that link with guests or generate a QR code for it.

## Game behavior
- Seven chapters, automatic answer checking, hints, progress bar, dated seals, final code and printable certificate.
- Each solved chapter earns 100 points; each hint costs 25 points (score cannot fall below zero).
- Progress is saved in the current browser using `localStorage` without an account. It will not automatically transfer to another device and can disappear if browser data is cleared.
- No backend or global leaderboard. This is an honor-system game: guests can inspect answers in source code. For secure scoring, guide-controlled access, shared leaderboard, and tamper-resistant answer validation, add a server/database later.
- The architectural-plan and mosaic puzzles are functional simplified text-based versions. Commission accurate illustrated interactive assets and route-test before commercial launch.
- The story and fictional ledger are fiction; opening dates should be verified before public use.
- For accessibility, all puzzle interactions use buttons instead of drag-only mechanics. High contrast and labeled progress included.

## Privacy
No guest account or personal data is transmitted by this page. The optional team name remains on the device. GitHub Pages hosting may process standard request logs under GitHub's policies.
