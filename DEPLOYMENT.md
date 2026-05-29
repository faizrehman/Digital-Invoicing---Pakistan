# Deployment Guide — DigitalInvoicing247 Marketing Website

## Overview

This static website deploys to **Cloudflare Pages** via GitHub integration.
DNS remains in **AWS Lightsail** — no domain transfer required.

---

## Step 1: Push to GitHub

### Option A — New repository (recommended)
```bash
# In the website/ folder
git init
git add .
git commit -m "Initial commit: DigitalInvoicing247 marketing website"

# Create repo on github.com then:
git remote add origin https://github.com/YOUR_USERNAME/digitalinvoicing247-website.git
git branch -M main
git push -u origin main
```

### Option B — Subdirectory of existing repo
Push the entire project (with `website/` folder) to GitHub.
When setting up Cloudflare Pages, set the **Root directory** to `website`.

---

## Step 2: Deploy on Cloudflare Pages

1. Log in at **dash.cloudflare.com**
2. Go to **Workers & Pages → Create application → Pages → Connect to Git**
3. Select your GitHub repository
4. Configure the build settings:

   | Setting | Value |
   |---|---|
   | Project name | `digitalinvoicing247` |
   | Production branch | `main` |
   | Build command | *(leave blank — static site)* |
   | Build output directory | `/` (or `website` if using subdirectory) |
   | Root directory | `/` (or `website` if using subdirectory) |

5. Click **Save and Deploy**

After first deploy, Cloudflare will give you a URL like:
`https://digitalinvoicing247.pages.dev`

---

## Step 3: Add Custom Domain (keep DNS in AWS)

### In Cloudflare Pages:
1. Go to your Pages project → **Custom domains** → **Set up a custom domain**
2. Enter: `www.digitalinvoicing247.com`
3. Cloudflare will show you a **CNAME record** target like:
   `digitalinvoicing247.pages.dev`

### In AWS Lightsail DNS (your current DNS manager):
1. Go to AWS Lightsail → **Networking** → Your domain DNS zone
2. Add a **CNAME record**:
   - **Name/Host**: `www`
   - **Value/Points to**: `digitalinvoicing247.pages.dev`
   - **TTL**: 300

3. For the apex domain (`digitalinvoicing247.com` without www):
   - Add a **CNAME record** (if your DNS supports CNAME at apex / ALIAS):
     - **Name**: `@` or leave blank
     - **Value**: `digitalinvoicing247.pages.dev`
   - OR add an **A record** pointing to Cloudflare's IP (not recommended — use www redirect)

### Recommended: Redirect apex → www
Add this `_redirects` file to your website folder:
```
https://digitalinvoicing247.com/* https://www.digitalinvoicing247.com/:splat 301
```

---

## Step 4: SSL Certificate

Cloudflare Pages automatically provisions a free SSL certificate for your custom domain once DNS propagates (usually within 1–24 hours).

---

## Updating the Website

Every `git push` to the `main` branch automatically triggers a new deployment on Cloudflare Pages. No manual steps needed after the initial setup.

---

## File Structure

```
website/
├── index.html          # Main landing page
├── css/
│   └── style.css       # All styles
├── js/
│   └── main.js         # Interactivity
├── assets/
│   ├── logo.png        # Brand logo (PNG)
│   ├── logo.svg        # Brand logo (SVG)
│   └── favicon.svg     # Favicon
├── _headers            # Cloudflare security headers
├── robots.txt          # SEO
├── sitemap.xml         # SEO sitemap
└── DEPLOYMENT.md       # This file
```

---

## Environment Details

- **Live app (primary)**: https://app.digitalinvoicing247.com/
- **Live app (secondary)**: https://app2.digitalinvoicing247.com/
- **Marketing website (target)**: https://www.digitalinvoicing247.com/
- **Cloudflare Pages preview**: https://digitalinvoicing247.pages.dev

---

## Contacts

- Sales: info@digitalinvoicing247.com | +92 333 9628527
- Support: support@digitalinvoicing247.com | +971 56 5349539
