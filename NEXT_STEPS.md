# Next Steps: Local Development & GitHub Pages Deployment

This document guides through testing the redesigned website locally and deploying it
to GitHub Pages. The site is located at:

```
/Users/shahab/Downloads/website/shahab-site/
```

---

## Table of Contents

1. [Prerequisites](#1-prerequisites) 
2. [Local Development](#2-local-development)
3. [Optional Future Improvements](#3-optional-future-improvements)
4. [Deploy to GitHub Pages](#4-deploy-to-github-pages)
5. [Custom Domain & SSL](#5-custom-domain--ssl)
6. [Ongoing Updates](#6-ongoing-updates)

---

## 1. Prerequisites

Ruby, Bundler, and all site dependencies are already installed and working on this machine.
System Ruby 2.6.10 is in use.

<details>
<summary>Setup instructions (if starting on a new machine)</summary>

### Install Ruby and Bundler (macOS)

Jekyll requires Ruby. The recommended way on macOS is via [rbenv](https://github.com/rbenv/rbenv):

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install rbenv and ruby-build
brew install rbenv ruby-build

# Add rbenv to shell (add to ~/.zshrc)
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc

# Install Ruby (3.2.x recommended for GitHub Pages compatibility)
rbenv install 3.2.2
rbenv global 3.2.2

# Verify
ruby --version

# Install bundler
gem install bundler
```

### Install Site Dependencies

```bash
cd /Users/shahab/Downloads/website/shahab-site
bundle install
```

> **Note:** This reads the `Gemfile` and installs `github-pages` and all required gems.
> The Gemfile pins `ffi < 1.16` for compatibility with macOS system Ruby (2.6.x).

</details>

---

## 2. Local Development

### Start the Local Server

```bash
cd /Users/shahab/Downloads/website/shahab-site
bundle exec jekyll serve --livereload
```

Then open: [http://localhost:4000](http://localhost:4000)

The site auto-rebuilds when you save files.

### Build Only (no server)

```bash
bundle exec jekyll build
```

---

## 3. Optional Future Improvements

- Add contact form
- Fine-tune `_config.yml` with your GitHub Pages URL once deployed
- Consider adding project thumbnails to the publications page

---

## 4. Deploy to GitHub Pages

### Step 1: Create a GitHub Repository

1. Go to [github.com](https://github.com) and create a new repository
2. Name it `yourusername.github.io` (for a user page) OR any name (for a project page)

### Step 2: Update `_config.yml`

For a **user page** (yourusername.github.io):
```yaml
url: "https://yourusername.github.io"
baseurl: ""
```

For a **project page** (yourusername.github.io/shahab-site):
```yaml
url: "https://yourusername.github.io"
baseurl: "/shahab-site"
```

### Step 3: Push to GitHub

```bash
cd /Users/shahab/Downloads/website/shahab-site
git init
git add .
git commit -m "Initial site build"
git branch -M main
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

### Step 4: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Select `main` branch and `/ (root)` folder
5. Click **Save**

Your site will be live at `https://yourusername.github.io` within a few minutes.

---

## 5. Custom Domain & SSL

### Using One Custom Domain (e.g., sagheb.net)

1. Add a `CNAME` file to the site root containing your domain:
   ```
   sagheb.net
   ```
2. Update `_config.yml`:
   ```yaml
   url: "https://sagheb.net"
   baseurl: ""
   ```
3. In your DNS provider (NameCheap), add these records:

   | Type  | Host | Value               |
   |-------|------|---------------------|
   | A     | @    | 185.199.108.153     |
   | A     | @    | 185.199.109.153     |
   | A     | @    | 185.199.110.153     |
   | A     | @    | 185.199.111.153     |
   | CNAME | www  | yourusername.github.io |

4. In GitHub → Settings → Pages, enter your custom domain and click Save.

### Adding a Second Domain (e.g., shahab.fyi in addition to sagheb.net)

GitHub Pages only supports **one** custom domain per repository — you cannot serve from
both `sagheb.net` and `shahab.fyi` simultaneously. The practical approach:

- Pick one as your **primary** (e.g., `sagheb.net`) — this goes in the `CNAME` file and
  is configured with the GitHub Pages A records above.
- Use NameCheap's **URL Redirect** feature on the second domain (`shahab.fyi`) to forward
  all traffic to the primary:
  - In NameCheap DNS for `shahab.fyi`, add a **URL Redirect Record**:
    - Host: `@` → redirects to `https://sagheb.net` (301 Permanent)
    - Host: `www` → redirects to `https://sagheb.net`
  - This costs nothing extra and takes effect within minutes.

If you later want `shahab.fyi` to be the primary, simply swap which domain is in the `CNAME`
file and `_config.yml`, and redirect `sagheb.net` → `shahab.fyi` instead.

### SSL / HTTPS — No Action Required

**GitHub Pages provides free, automatic HTTPS via [Let's Encrypt](https://letsencrypt.org/).**
You do not need to manually obtain or renew SSL certificates — this replaces the annual
PositiveSSL process you currently do through NameCheap/cPanel.

Once your custom domain DNS has propagated (can take up to 48 hours, usually minutes):
1. Go to your repo → **Settings** → **Pages**
2. Check **"Enforce HTTPS"** (GitHub will have auto-issued a certificate by this point)
3. Done — GitHub renews the certificate automatically every 90 days, forever, at no cost.

> If the "Enforce HTTPS" checkbox is greyed out, wait a few more minutes for DNS propagation
> and refresh the page. The Let's Encrypt certificate is issued automatically once GitHub
> can verify your DNS records point to their servers.

---

## 6. Ongoing Updates

### Adding a New Publication

Edit `_data/publications.yml` — add an entry following the existing format. Also update `_data/years.yml` if it's a new year.

### Adding a New Course

Edit `_data/teaching.yml` — add an entry. Create a corresponding sub-page at `teaching-NEW-COURSE.html`.

### Adding a New Practice Project

Edit `_data/practice.yml` — add an entry. Create a corresponding sub-page at `practice-NEW-PROJECT.html`.

### Updating the About Page

Edit `about.html`.

### Changing Site Title or Author

Edit `_config.yml` — update `title`, `author`, `email`, `description`.
