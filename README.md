# Jackson Creative + Jackson Insight — Website

A single static site (HTML/CSS/JS, no build step) that hosts both brands with a top-bar toggle between them.

- **Creative** lives at the site root (`index.html`, `services.html`, `work.html`, `contact.html`).
- **Insight** lives under `insight/` (`index.html`, `services.html`, `work.html`, `contact.html`, `text-signup.html`, `privacy.html`, `terms.html`).
- Shared styles and scripts are in `assets/`. Logos and partner logos are in `assets/` and `assets/partners/`.

Hosting: **Vercel** · Source control: **GitHub** · Editing: locally on your Mac, or in Claude Code cloud sessions (which work on a `claude/…` branch and open a pull request).

---

## One-time setup

### 0. Prerequisites
- Git installed (`git --version`). On macOS it comes with the Xcode command line tools: `xcode-select --install`.
- A GitHub account and a Vercel account (log into Vercel with your GitHub account for the smoothest flow).
- Optional but handy: the GitHub CLI (`brew install gh`) and the Vercel CLI (`npm i -g vercel`).

### 1. The repo is already initialized and committed
The setup tool already ran `git init` and made the first commit on the `main` branch, so you can skip straight to pushing. Confirm from Terminal:

```bash
cd "$HOME/Documents/Claude/Projects/Jackson Creative Website"
git log --oneline
```

You should see the "Initial commit" line. If you ever need to start the history over, run `rm -rf .git` then `git init && git add -A && git commit -m "Initial commit"`.

Paste one command at a time. Do not paste the explanatory notes — only the lines inside the grey code boxes.

### 2. Create the GitHub repository and push

**Option A — manually (no extra tools needed):**
1. Go to https://github.com/new and create a new **empty** repo named `jackson-creative-website`. Leave "Add a README", ".gitignore", and "license" all **unchecked** — this folder already has them.
2. Then, from Terminal in this folder, replace `YOUR_USERNAME` with your GitHub username and run:
```bash
git remote add origin https://github.com/YOUR_USERNAME/jackson-creative-website.git
git push -u origin main
```

**Option B — using the GitHub CLI (optional):** if you'd rather not use the website, install the CLI first with `brew install gh` (requires Homebrew), then:
```bash
gh auth login
gh repo create jackson-creative-website --private --source=. --remote=origin --push
```

### 3. Import the repo into Vercel
1. Go to vercel.com → **Add New… → Project** → import `jackson-creative-website` from GitHub.
2. Framework preset: **Other** (it's a static site). Leave **Build Command** empty and **Output Directory** empty/root.
3. Click **Deploy**. You'll get a live `*.vercel.app` URL in a minute.

### 4. Add your domains
In the Vercel project → **Settings → Domains**, add both:
- `thejacksoncreative.com` (and `www.thejacksoncreative.com`) — your primary domain.
- `jacksoninsight.com` (and `www.jacksoninsight.com`) — also point this at the **same** project.

`vercel.json` already handles the Insight redirect: when a visitor lands on `jacksoninsight.com`, the root and the key pages redirect into the `/insight` section:
- `jacksoninsight.com` → `/insight`
- `jacksoninsight.com/text-signup` → `/insight/text-signup`
- `jacksoninsight.com/privacy` → `/insight/privacy`
- `jacksoninsight.com/terms` → `/insight/terms`
- `jacksoninsight.com/contact` → `/insight/contact`

**DNS:** at your domain registrar, point each domain to Vercel as Vercel instructs (usually an `A` record to `76.76.21.21` for the apex, and a `CNAME` to `cname.vercel-dns.com` for `www`). Vercel shows the exact records per domain and issues SSL automatically.

---

## Everyday workflow (push changes to the live site)

Edit files locally, then from Terminal in this folder:

```bash
git add -A
git commit -m "Describe what you changed"
git push
```

Vercel automatically builds and deploys every push to `main` — your changes go live in about a minute. Each push also gets its own preview URL, and the Vercel dashboard lets you roll back to any previous deploy instantly.

Tip: work on a branch for bigger changes and open a Pull Request — Vercel posts a preview URL on the PR so you can review before merging to `main`.

---

## Clean URLs

Clean URLs are **off**: `vercel.json` sets `cleanUrls: false` (turned off to fix 404s on the Insight footer links). Pages are served at their `.html` paths — `/services.html`, `/work.html`, `/insight/text-signup.html`, etc. — and internal links use those full paths. If you turn clean URLs back on, re-check every Insight link before pushing.

**For the 10DLC SMS submission**, your sign-up page is at:
`https://www.thejacksoncreative.com/insight/text-signup.html` and (via redirect) `https://www.jacksoninsight.com/text-signup`.
Privacy and Terms are at `/insight/privacy.html` and `/insight/terms.html` (and the `jacksoninsight.com/privacy` · `/terms` shortcuts).

---

## Hero mosaics

- **Creative** (`index.html`): a 4×3 grid of tiles behind the hero. Four tiles loop muted video clips — drop them in as `assets/hero-grid/clip1.mp4` … `clip4.mp4` (optional matching `.webm`), commit, and push. Until a clip exists its tile shows a still poster. See `assets/hero-grid/README.txt`.
- **Insight** (`insight/index.html`): still tiles only, using the data-viz set in `assets/insight-grid/` (`tile1–tile6.svg`). Replace those files to change the look.

---

## Form capture

All three forms post to **Formspree**:

| Form | File | Formspree form |
|---|---|---|
| Creative contact | `contact.html` | `mlgqyrpb` |
| Insight contact | `insight/contact.html` | `mwvgderj` |
| Insight SMS sign-up | `insight/text-signup.html` | `mwvgderj` (shared with Insight contact) |

To change where a form goes, replace its `action="https://formspree.io/f/…"` URL.

**Option for later — Supabase (own your data):** create a Supabase project, add a Vercel Serverless Function under `/api` that inserts submissions into a table, and store the Supabase URL/key as Vercel Environment Variables (never commit them). Useful if you want a stored, timestamped record of SMS opt-in consent. Ask and I can scaffold this.

---

## Project structure

```
.
├── index.html            # Creative — home
├── services.html         # Creative — services
├── work.html             # Creative — work
├── contact.html          # Creative — contact
├── insight/
│   ├── index.html        # Insight — home
│   ├── services.html
│   ├── work.html
│   ├── contact.html
│   ├── text-signup.html  # SMS opt-in sign-up (10DLC)
│   ├── privacy.html      # Privacy Policy (SMS section)
│   └── terms.html        # Terms & Conditions (SMS section)
├── assets/
│   ├── styles.css        # shared styles + themes
│   ├── script.js         # shared interactions
│   ├── jackson-creative-logo.png
│   ├── jackson-insight-logo.png
│   ├── hero-grid/        # Creative hero clips (clip1–4.mp4) + README
│   ├── insight-grid/     # Insight hero tiles (tile1–6.svg)
│   ├── work/             # Selected Work card images
│   └── partners/         # partner logos
├── vercel.json           # jacksoninsight.com redirects (cleanUrls off)
├── .gitignore
└── README.md
```
