# Making the trip dashboard collaborative — setup guide

Your `parvati-trip-dashboard.html` file now has collaborative mode built in and
ready to go. It's off by default (everyone just gets their own local copy, like
before). Follow the steps below once, and everyone who opens the shared link
will see the same board update live — taps, edits, stars, all of it.

**Time needed:** ~15 minutes. **Cost:** free, for a trip-planning-sized group.

---

## How it works, in one paragraph

The page now talks to a small free Google service called **Firebase Realtime
Database**. When you (or a friend) tap a box, star a café, or add a day, the
page writes that one small change to Firebase. Every other open copy of the
page is listening for changes and updates itself instantly — no refresh
needed. GitHub Pages still just serves the HTML file exactly as before;
Firebase is the "shared whiteboard" behind it.

---

## Step 1 — Create a Firebase project

1. Go to **[console.firebase.google.com](https://console.firebase.google.com)**
   and sign in with any Google account.
2. Click **Add project** (or **Create a project**).
3. Give it a name — e.g. `parvati-trip-planner`. Firebase will generate a
   project ID like `parvati-trip-planner-xxxxx`.
4. Disable Google Analytics for this project (not needed) and click
   **Create project**. Wait ~30 seconds for it to finish.

## Step 2 — Turn on Anonymous sign-in

This lets your friends open the link and start collaborating instantly —
no account, no password — while still keeping randos off the internet out.

1. In the left sidebar, go to **Build → Authentication**.
2. Click **Get started**.
3. Under **Sign-in method**, click **Anonymous** in the provider list.
4. Toggle **Enable**, then **Save**.

## Step 3 — Create the Realtime Database

1. In the left sidebar, go to **Build → Realtime Database**.
2. Click **Create Database**.
3. Pick a location close to you/your friends (any is fine — this only
   affects latency slightly).
4. Choose **Start in locked mode** (we'll set proper rules in the next step).
5. Click **Enable**.

## Step 4 — Set the security rules

1. Still in **Realtime Database**, click the **Rules** tab.
2. Replace whatever is there with exactly this:

   ```json
   {
     "rules": {
       "trips": {
         "$tripId": {
           ".read": "auth != null",
           ".write": "auth != null"
         }
       }
     }
   }
   ```

3. Click **Publish**.

   This means: anyone who's signed in (even anonymously, from Step 2) can
   read and write trip data, but nobody outside Firebase can. Since the
   trip link includes a random, hard-to-guess trip code, this is a
   reasonable level of privacy for a friend-group trip planner. Don't use
   this pattern for anything sensitive (passwords, payment info, etc.) —
   it's built for "plan a weekend trip with friends," not secure data.

## Step 5 — Register a web app to get your config keys

1. Go to **Project settings** (the gear icon, top left, next to "Project
   Overview").
2. Scroll to **Your apps** and click the **`</>`** (web) icon.
3. Give it a nickname (e.g. `trip-dashboard`) — you don't need Firebase
   Hosting, just click **Register app**.
4. Firebase will show you a code block that looks like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "parvati-trip-planner-xxxxx.firebaseapp.com",
     databaseURL: "https://parvati-trip-planner-xxxxx-default-rtdb.firebaseio.com",
     projectId: "parvati-trip-planner-xxxxx",
     ...
   };
   ```

   **Copy these four values** — `apiKey`, `authDomain`, `databaseURL`,
   `projectId`. You'll paste them into the HTML file next.

   > ⚠️ Your **Realtime Database** region matters here. If you picked a
   > region other than `us-central1` in Step 3, your `databaseURL` will
   > look like `https://PROJECT-default-rtdb.REGION.firebasedatabase.app`
   > instead. Always copy the exact URL Firebase shows you — don't guess it.

## Step 6 — Paste your config into the HTML file

1. Open `parvati-trip-dashboard.html` in any text editor (or GitHub's
   built-in web editor — see below).
2. Find this block near the top of the `<script>` section (search for
   `FIREBASE_CONFIG`):

   ```js
   var FIREBASE_CONFIG = {
     apiKey: "PASTE_YOUR_API_KEY",
     authDomain: "PASTE_YOUR_PROJECT_ID.firebaseapp.com",
     databaseURL: "PASTE_YOUR_DATABASE_URL",
     projectId: "PASTE_YOUR_PROJECT_ID"
   };
   ```

3. Replace the four placeholder strings with your real values from Step 5.
   It should now look like:

   ```js
   var FIREBASE_CONFIG = {
     apiKey: "AIzaSyABC123...",
     authDomain: "parvati-trip-planner-xxxxx.firebaseapp.com",
     databaseURL: "https://parvati-trip-planner-xxxxx-default-rtdb.firebaseio.com",
     projectId: "parvati-trip-planner-xxxxx"
   };
   ```

4. Save the file.

**To edit directly on GitHub** (no local setup needed): open the file in
your repo on github.com, click the pencil (✎) **Edit** icon, make the
change, then **Commit changes** at the bottom.

## Step 7 — Push to GitHub and wait for Pages to redeploy

If editing locally:

```bash
git add parvati-trip-dashboard.html
git commit -m "Enable collaborative mode"
git push
```

If you edited on github.com directly, this already happened when you
committed in Step 6.

GitHub Pages usually redeploys within **30–90 seconds**. You can check
progress under your repo's **Actions** tab (look for the "pages build and
deployment" run to turn green).

## Step 8 — Test it

1. Open your GitHub Pages URL in one browser tab.
2. Open the **same URL** in a private/incognito window (or a different
   browser, or your phone).
3. At the top of the page, you should see a small status dot turn **green**
   with "Live — syncing with your group" within a few seconds. If it does,
   Firebase is connected correctly.
4. Tap "Tap to lock" on any day in one window — it should light up in the
   other window within a second or two, with no refresh.

If the dot stays grey ("Local only"), your config wasn't picked up — double
check Step 6. If it turns red ("Couldn't connect"), open your browser's
console (F12 → Console tab) and check the error — it's almost always either
Anonymous auth not enabled (Step 2) or the database rules not published
(Step 4).

## Step 9 — Share it with your friends

- Open your published GitHub Pages URL.
- Click the **🔗 Copy invite link** button near the top of the page. This
  copies a link with a unique trip code, like
  `https://you.github.io/repo/?trip=a1b2c3`.
- Send that exact link to your friends (WhatsApp, etc.) — not the bare
  URL. The `?trip=a1b2c3` part is what puts everyone on the same shared
  board. Anyone who opens a link *without* a trip code gets a fresh, new,
  private trip.
- Everyone should type their name in the "You're: ___" field at the top so
  you can see who's currently looking at the plan.

---

## What you get

- **Live sync** — a tap, star, edit, or new day added by anyone shows up
  for everyone within a second or two, no refresh.
- **Presence** — see who's currently on the page.
- **Still works offline/solo** — if you ever remove the Firebase config,
  the page quietly falls back to saving locally in each person's own
  browser, exactly like before.
- **Multiple trips** — anyone can start a brand-new trip board just by
  opening the link without a `?trip=` code (or by editing the URL to a new
  code), so this one page can be reused for future trips too.

## Known limitation

If two people edit the **exact same text field** (say, the trip notes box,
or the exact same day's description) within the same second, the last one
to finish typing wins — there's no fancy merge. Taps (locking in a day,
starring a place) never conflict like this since each is tracked
individually. For a small trip-planning group this is very unlikely to
matter in practice.

## Costs

Firebase's free "Spark" plan includes 1 GB of Realtime Database storage and
10 GB/month of downloaded data — a trip planner like this one uses a few
kilobytes per trip. You will not hit the free tier limits for this use case.

## Troubleshooting quick reference

| Symptom | Likely cause | Fix |
|---|---|---|
| Status dot stays grey | Config not saved/pushed | Re-check Step 6, confirm the file on GitHub has your real keys |
| Status dot turns red | Auth or rules issue | Re-check Steps 2 and 4 |
| "Permission denied" in console | Rules not published, or Anonymous auth off | Steps 2 & 4 |
| Changes don't appear on the other device | Different trip codes | Make sure both links have the same `?trip=...` |
| Works for you, not your friend | They opened the bare link (new trip created) | Re-send the exact link with `?trip=...` from the Copy invite link button |

---

If you'd like, I can also help you set up a custom short link (e.g. via
Bit.ly) so the invite link is easier to share, or add a "rename this trip"
field so the trip code isn't the only identifier. Just ask.
