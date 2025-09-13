# Client Preview Template (GitHub Pages)

This repo is a **starter template** for hosting a client preview at a custom subdomain like:
```
clientX.hosting.doxservices.com
```
It assumes the parent domain DNS has a wildcard CNAME:
```
*.hosting.doxservices.com  CNAME  username.github.io
```
> Replace `username.github.io` with your GitHub Pages apex.

---

## Quick Start

1. **Enable Pages** in this repo: Settings → Pages → Build and deployment → Source = GitHub Actions.
2. **Create `CNAME`** file in repo root with your hostname, e.g.:
```
clientX.hosting.doxservices.com
```
3. **Push to `main`**. The included workflow deploys the site.
4. **Force HTTPS** in Pages settings.
5. Keep **noindex** until launch: there's a `robots.txt` and `<meta name="robots" content="noindex">` in pages.

### Directory Layout
```
/
├─ index.html
├─ about.html
├─ contact.html
├─ privacy.html
├─ styleguide.html
├─ 404.html
├─ robots.txt          # default Disallow all (flip on launch)
├─ CNAME               # add your subdomain (one line)
├─ assets/
│  ├─ css/style.css
│  └─ js/main.js
└─ .github/workflows/pages.yml
```

---

## Launch Checklist

- [ ] `CNAME` contains `clientX.hosting.doxservices.com`
- [ ] GitHub Pages → Force HTTPS ON
- [ ] `robots.txt` switched to allow indexing when approved (see below)
- [ ] Remove `<meta name="robots" content="noindex, nofollow">` lines from HTML
- [ ] Optional: Add Cloudflare proxy/WAF and Access policy
- [ ] Optional: Custom 404/maintenance copy

### Allow indexing at launch
- Replace `robots.txt` with:
  ```
  User-agent: *
  Disallow:
  ```
- Remove the `<meta name="robots" content="noindex, nofollow">` from HTML files.

---

## Cloudflare (optional but recommended)

**Proxy the DNS** for `hosting.doxservices.com` (orange cloud) to add WAF/caching and hide the GitHub origin.

**Access (Zero Trust) rule** example to require email OTP (per-client hostname match):

- **Include**: Hostname equals `clientX.hosting.doxservices.com`
- **Action**: Allow
- **Authentication**: One-time pin, Email — allowed emails `{client@example.com, you@doxservices.com}`
- **Default**: Block (rule order matters)

**Transform Rule** (temporary noindex header at edge):
- When: Hostname matches `*.hosting.doxservices.com`
- Then add response header: `X-Robots-Tag: noindex, nofollow`

Remove when launching publicly.

---

## Security Notes

- GitHub Pages serves public artifacts. Keep sensitive assets out of the repo.
- To keep the code private pre-launch: make the repository **private** (Pages can still publish the output).
- Use randomized preview URLs and do not link publicly until approved.
- Consider separate repos per client for isolation and clean hand-off.

---

## Branding

This template ships with a neutral, accessible design. Swap fonts/colors in `assets/css/style.css`. All assets use **relative paths** so you can rename the subdomain or move the repo without breaking links.

---

_Last updated: 2025-09-13_
