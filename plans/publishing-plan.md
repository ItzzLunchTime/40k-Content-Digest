# Publishing Plan — 40k Weekly Digest HTML
**Project:** 40k Weekly Digest  
**Status:** Active — using GitHub Pages  
**Last updated:** 2026-05-22

---

## Current setup

The weekly HTML digest is published to GitHub Pages automatically each Monday as part of the scheduled task. The live URL is:

**https://itzzlunchtime.github.io/40k-Content-Digest/**

The repo is public. No password protection. Only people you share the URL with are likely to find it (it won't appear in search results).

---

## How it works

1. Claude researches and generates `digest-YYYY-MM-DD.html` each Monday
2. The same content is saved as `index.html` (always the current week at the root URL)
3. Claude pushes both files to the GitHub repo via git
4. GitHub Pages deploys automatically within ~30 seconds
5. All historic digests remain accessible at their dated URLs

---

## URL structure

| URL | Content |
|-----|---------|
| `https://itzzlunchtime.github.io/40k-Content-Digest/` | Always the current week |
| `https://itzzlunchtime.github.io/40k-Content-Digest/digest-2026-05-22.html` | Specific week by date |

---

## GitHub token

The scheduled task uses a Personal Access Token to authenticate git pushes. Tokens should be rotated periodically.

To generate a new token:
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token → check `repo` scope → copy token
3. Tell Claude: "Here is my new GitHub token: [token]" — Claude will update the scheduled task

---

## Future enhancement: digest index page

Once several weeks of data have accumulated, add an `archive.html` page listing all past digests as clickable links. Claude can generate and update this as part of the Monday task.

---

## Alternative: private access

If you ever want to restrict access, the two best options are:
- **Netlify Pro** ($19/month) — password protection, MCP connector for automation
- **Cloudflare Pages + Access** ($0) — email-gated via one-time login link

Both options are detailed in the original publishing plan notes. GitHub Pages itself cannot be made private without GitHub Enterprise pricing.
