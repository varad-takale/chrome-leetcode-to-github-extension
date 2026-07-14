# LeetCode → GitHub Sync

A Chrome extension that automatically commits your code to a GitHub repo the moment a LeetCode submission is **Accepted**. No manual copy-pasting.

## How it works
1. `inject.js` runs in the LeetCode page itself and watches for the "Accepted" result.
2. `content.js` then fetches the full submission (code, language, difficulty) via LeetCode's own GraphQL API.
3. `background.js` pushes that file straight to your GitHub repo using the GitHub REST API, and keeps a `README.md` table of solved problems up to date.

Nothing leaves your machine except the direct call to `api.github.com` — there's no third-party server in between.

## Install it (load unpacked)
1. Unzip this folder somewhere permanent (don't delete it later — Chrome loads the extension from this location).
2. Go to `chrome://extensions`.
3. Turn on **Developer mode** (top-right toggle).
4. Click **Load unpacked** and select this folder.
5. Pin the extension (puzzle-piece icon → pin) so it's easy to reach.

## One-time setup
1. Create a GitHub **Personal Access Token**:
   - Fine-grained token → give it **Contents: Read and write** access on the repo you want to use, or
   - Classic token → check the `repo` scope.
   - The popup has a "Create one →" link that pre-fills the scope for you.
2. Create the GitHub repo you want solutions pushed to (can be empty).
3. Click the extension icon → fill in:
   - **GitHub Personal Access Token**
   - **Owner** (your GitHub username or org)
   - **Repository** (e.g. `leetcode-solutions`)
   - **Branch** (defaults to `main`)
4. Click **Save settings**.

## Use it
Just solve problems on LeetCode normally. When a submission comes back **Accepted**, the extension:
- Writes the solution to `<Difficulty>/<id>-<slug>.<ext>` (e.g. `Easy/1-two-sum.py`)
- Adds a short header comment with the problem link, runtime, and memory
- Adds/updates a row in the repo's `README.md`

Open the extension popup any time to see the status of the last sync attempt.

## Notes / limitations
- Works on `leetcode.com/problems/*` pages (not the old `leetcode-cn.com` domain).
- Your GitHub token is stored in `chrome.storage.sync` (synced across your signed-in Chrome browsers, not sent anywhere except to `api.github.com`).
- If GitHub's API or LeetCode's GraphQL schema changes shape, the relevant fetch calls in `background.js` / `content.js` are the place to patch.
- To revoke access, delete the token from your GitHub settings and it stops working immediately.
