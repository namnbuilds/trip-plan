Parvati Valley Trip Dashboard

This is a single-file static trip planner (HTML + JS + CSS). It supports:
- Editable route option headers (name / place / color)
- Click-to-lock itinerary cells
- Star (shortlist) stays and cafés
- Location thumbnails and Google Maps search links
- Persistent state (uses window.storage when available, or a fallback)

Hosting & sharing — step-by-step

1) Create a new GitHub repository and push this folder

  git init
  git add .
  git commit -m "Initial trip dashboard"
  git branch -M main
  git remote add origin https://github.com/YOURUSERNAME/YOURREPO.git
  git push -u origin main

2) Host on GitHub Pages (quick, free)
- In the repo, open Settings → Pages.
- Under "Build and deployment", choose "Deploy from a branch".
- Choose the `main` branch and root `/` folder, then Save.
- GitHub will publish at `https://YOURUSERNAME.github.io/YOURREPO/` within a minute.

3) Host on Vercel (automatic previews + easy collaboration)
- Sign in to Vercel (or create an account) and "Import Project" from GitHub.
- Select the repo and accept defaults (it's a static site). Vercel will auto-deploy.
- Pushes to `main` will trigger new deployments. Invite collaborators via the Vercel project settings.

4) Host on Netlify (drag-and-drop or Git-based)
- For a quick test, open https://app.netlify.com/drop and drag the single HTML file.
- For production: connect your GitHub repo in Netlify, configure build to "None (static)" and deploy the `main` branch.

5) Share & collaborate
- To let others edit the file and contribute, use the GitHub repo. Team members can submit pull requests.
- For non-technical contributors, consider adding a simple CONTRIBUTING.md that explains how to edit the HTML (or use GitHub's web editor) and submit changes.

6) Optional: use a tiny CMS or Google Sheet
- If you want non-technical live editing of locations, consider pairing this static site with a small JSON file stored in the repo (update via PR) or a Google Sheet + a tiny build step to export JSON.
- For real-time collaborative editing, a minimal backend (Firebase / Supabase) is needed. I can add an example if you want.

Local testing
- Open `kasol-trip-plan.html` in a browser (double-click or `open kasol-trip-plan.html` on macOS).
- Changes are persisted to the storage backend in the browser.

Need help?
If you want, I can:
- Create the Git repo and push this project for you (I will show commands you can run locally).
- Add a small `deploy` script for GitHub Pages.
- Wire a simple Netlify/Vercel config and test deployment instructions.

