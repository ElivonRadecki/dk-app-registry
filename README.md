# The Danish App Registry — Project Documentation

A practical index of apps, platforms, and digital services for newcomers to Denmark.
Built and maintained by Elivon Radecki.

**Live site:** https://elivonradecki.github.io/dk-app-registry/

---

## Table of Contents

1. [Project overview](#1-project-overview)
2. [Technology stack](#2-technology-stack)
3. [How the live feed works](#3-how-the-live-feed-works)
4. [Adding or editing apps](#4-adding-or-editing-apps)
5. [Pushing code changes to the site](#5-pushing-code-changes-to-the-site)
6. [Community suggestions workflow](#6-community-suggestions-workflow)
7. [Analytics](#7-analytics)
8. [Accounts and credentials](#8-accounts-and-credentials)
9. [Troubleshooting](#9-troubleshooting)

---

## 1. Project overview

The Danish App Registry is a free, independent, community-supported reference guide. It lists over 100 apps and digital services across 18 categories, each with a description, platform availability, requirements, priority level, and a link.

The site is hosted on GitHub Pages (free), reads its data live from a Google Sheet, and requires no server or paid infrastructure beyond a free Cloudflare Worker proxy.

**Key design decisions:**
- Content lives in Google Sheets — no coding needed to add or edit apps
- The HTML file contains only the layout and logic, not the data
- A Cloudflare Worker acts as a CORS proxy so the site can fetch from Google Sheets
- GoatCounter provides privacy-friendly analytics with no cookie banner needed
- A Google Form collects community suggestions by email

---

## 2. Technology stack

| Component | Service | Cost |
|---|---|---|
| Hosting | GitHub Pages | Free |
| Data source | Google Sheets (published as CSV) | Free |
| CORS proxy | Cloudflare Workers | Free |
| Analytics | GoatCounter | Free |
| Suggestion form | Google Forms | Free |
| Version control | GitHub + GitHub Desktop | Free |
| Domain | github.io subdomain | Free |

**Total running cost: €0/month**

---

## 3. How the live feed works

When someone visits the site, this happens:

1. The browser loads `index.html` from GitHub Pages
2. The JavaScript in `index.html` makes a fetch request to the Cloudflare Worker
3. The Cloudflare Worker fetches the Google Sheet CSV and returns it with CORS headers
4. The JavaScript parses the CSV and renders the app cards

This means **content changes in Google Sheets go live on the website within 1–2 minutes** with no code push needed.

### Key URLs

| Resource | URL |
|---|---|
| Live website | https://elivonradecki.github.io/dk-app-registry/ |
| GitHub repository | https://github.com/ElivonRadecki/dk-app-registry |
| Google Sheet | https://docs.google.com/spreadsheets/d/e/2PACX-1vTUTqmi_EtItH6PlhjoT790bG9ulEDO6R9jqz0ko3k9cUSFgmULC12Bw15UoXDXmBB_vPubdct4o4bW/pub?gid=1288350490&single=true&output=csv |
| Cloudflare Worker | https://myevr.danishappregistry.workers.dev |
| GoatCounter dashboard | https://danishappregistry.goatcounter.com |
| Google Form (suggestions) | https://docs.google.com/forms/d/e/1FAIpQLSd9MVO4VgUyLhwqv8v-SBEG-LZ7BGkduszm-k_0GmoZGMaBdw/viewform |

---

## 4. Adding or editing apps

All content is managed in the **Google Sheet**. No code editing required.

### Google Sheet columns

| Column | Description |
|---|---|
| Name | App name as it appears on the site |
| Category | Must match exactly one of the 18 categories (see below) |
| Description | Full description shown on the card |
| Platforms | Semicolon-separated: `iOS; Android; Web` |
| Requirements | Semicolon-separated: `CPR number; MitID` |
| Priority | Must-have / Essential / Useful / Optional |
| URL | Full URL including https:// |
| Visible (TRUE/FALSE) | TRUE to show, FALSE to hide without deleting |
| Notes / Community suggestions | Internal notes, not shown on site |

### Valid categories (must match exactly)

- Digital Identity & Government
- Health
- Payments & Banking
- Transport
- Libraries & Culture
- Nature & Outdoors
- Events & Leisure
- Food & Shopping
- Danish Language & Classes
- News & Media
- Utilities & Home *(merged into Waste & Environment)*
- Waste & Environment
- Mental Health & Wellbeing
- Minorities & Community Support
- Accessibility & Special Needs
- Family & Children
- Work & Employment
- Housing

### Valid priority values

- Must-have
- Essential
- Useful
- Optional

### To add a new app

1. Open the Google Sheet
2. Add a new row at the bottom
3. Fill in all columns
4. Set Visible to TRUE
5. The site updates automatically within 1–2 minutes

### To temporarily hide an app

Change the Visible column from TRUE to FALSE. The app disappears from the site but stays in the sheet.

### To permanently remove an app

Delete the row from the sheet.

---

## 5. Pushing code changes to the site

Code changes (design, layout, new features) require editing `index.html` and pushing to GitHub. Content changes (apps, descriptions) only require editing the Google Sheet.

### Workflow using GitHub Desktop

1. Make sure the local repo folder exists at `/Users/eli/Documents/GitHub/dk-app-registry`
2. Replace `index.html` in that folder with the updated version
3. Open **GitHub Desktop** — it will detect the change automatically
4. Type a short summary in the **Summary** field (e.g. "Update: add new categories")
5. Click **Commit to main**
6. Click **Push origin**
7. Wait ~60 seconds for GitHub Pages to redeploy
8. Hard refresh the live site with **Cmd+Shift+R**

### If GitHub Desktop says "Can't find dk-app-registry"

The local folder was moved or deleted. Click **Locate...** to find it, or **Clone Again** to re-download from GitHub. After cloning, replace `index.html` with the latest version before committing.

---

## 6. Community suggestions workflow

### How suggestions arrive

1. A visitor fills in the Google Form at the bottom of the page
2. You receive an email notification immediately
3. Review the suggestion

### Adding a valid suggestion

For each approved app, Claude (AI assistant) can research and produce a ready-to-paste row with:
- Full description
- Platform availability
- Requirements
- Priority level
- URL

Just tell Claude: *"Add [app name] to the registry"* and paste the resulting row into the sheet.

### Google Form email notifications

To confirm notifications are active:
1. Go to forms.google.com → open the form
2. Click the three dots ⋮ menu
3. Confirm **Get email notifications for new responses** is ticked

---

## 7. Analytics

Traffic is tracked via **GoatCounter** — privacy-friendly, no cookies, GDPR-compliant.

- **Dashboard:** https://danishappregistry.goatcounter.com
- **Account:** danishappregistry@gmail.com
- Shows: page views, referral sources, countries, devices, browsers
- Data updates in real-time

---

## 8. Accounts and credentials

| Service | Account | Notes |
|---|---|---|
| GitHub | ElivonRadecki | Hosts the repository and live site |
| Google (project) | danishappregistry@gmail.com | Owns the Sheet, Form, and GoatCounter |
| Cloudflare | danishappregistry@gmail.com | Hosts the CORS proxy Worker |
| GoatCounter | danishappregistry@gmail.com | Analytics dashboard |

> **Note:** Keep these credentials stored securely. The Google account owns both the data source (Sheet) and the suggestion form. Loss of access to this account would require re-setting up the Sheet and updating the URL in `index.html`.

---

## 9. Troubleshooting

### Site shows "Could not load the registry"

The Cloudflare Worker fetch failed. Possible causes:

| Cause | Fix |
|---|---|
| Corporate network/VPN blocking the Worker | Disconnect from VPN, or test on phone |
| Cloudflare Worker is down | Check https://myevr.danishappregistry.workers.dev — if it returns "Missing ?url= parameter" it's working |
| Google Sheet not published | Go to File → Share → Publish to web in the Sheet and confirm it's published |
| Cached old version | Hard refresh with Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows) |

### Site loads but categories have no icons

A category name in the Google Sheet doesn't exactly match the categoryIcons object in `index.html`. Check for typos or extra spaces in the Category column. Category names are case-sensitive.

### GitHub Desktop can't find the repo

Click **Locate...** and navigate to the folder, or click **Clone Again** to re-download from GitHub. After cloning, replace `index.html` with the latest version from Claude before committing.

### Google Form not showing on the site

The form embed URL is hardcoded in `index.html`. If you delete and recreate the form, the URL changes and `index.html` must be updated with the new iframe embed code.

### Cloudflare Worker stopped working

Log in to cloudflare.com with danishappregistry@gmail.com → Workers & Pages → find the `myevr` worker → check it's active. If it was accidentally deleted, create a new Worker and paste the proxy code again (see section 3).

---

*Last updated: August 2026*
*Built with Claude (Anthropic)*
