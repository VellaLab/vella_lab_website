# Publish thevellalab.org from GitHub

This site is static HTML/CSS. Use **GitHub Pages** with the custom domain **thevellalab.org**.

## Files in this repository

| File | Purpose |
|------|--------|
| `CNAME` | Tells GitHub Pages the canonical hostname is `thevellalab.org`. Must stay in the **published root** (repository root if you deploy from `/`). |
| `.nojekyll` | Disables Jekyll so GitHub serves files as-is (avoids surprises with underscored paths or `README.md` processing). |
| `dns/github-pages-thevellalab.org.txt` | Human-readable list of DNS records to create at your registrar. **Not** deployed as part of the site. |

DNS itself is **not** stored in Git: you enter records in your domain registrar’s (or DNS host’s) control panel.

## 1. Push this repository to GitHub

If it is not already on GitHub, create a new repository and push the default branch (e.g. `main`).

## 2. Turn on GitHub Pages

1. Open the repository on GitHub → **Settings** → **Pages** (under *Code and automation*).
2. Under **Build and deployment** → **Source**, choose **Deploy from a branch**.
3. Select your default branch and folder **`/ (root)`**, then save.

After a few minutes, the site should be available at:

- **Project site:** `https://<user-or-org>.github.io/<repository-name>/`
- **User/org site** (only if the repo is named `<username>.github.io`): `https://<user-or-org>.github.io/`

## 3. Connect the custom domain on GitHub

1. Still under **Settings** → **Pages**, find **Custom domain**.
2. Enter **`thevellalab.org`** and save.

GitHub may open a pull request or commit that adds/updates `CNAME`; if you already have `CNAME` in the repo with the same name, you are aligned.

3. When DNS is correct, enable **Enforce HTTPS** (can take up to a day after DNS propagates).

Optional but recommended: [Verify your custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) in **Settings** → **Pages** to reduce takeover risk.

## 4. Configure DNS at your registrar

Use the values in `dns/github-pages-thevellalab.org.txt`.

**Apex (`thevellalab.org`):** add the four **A** records (and optionally the four **AAAA** records) to GitHub Pages’ IPs listed in that file.

**`www.thevellalab.org`:** add a **CNAME** from `www` to your GitHub Pages hostname, e.g. `your-org.github.io` (no `https://`, no path). See GitHub’s table: [DNS records for your custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#dns-records-for-your-custom-domain).

Propagation can take up to 24–48 hours.

## 5. Ongoing maintenance

- Edit HTML/CSS/images in this repo, commit, and push to the branch configured for Pages.
- Keep **`CNAME`** in the published root with exactly:

  ```text
  thevellalab.org
  ```

- If you use a GitHub Action to deploy to Pages instead of “Deploy from a branch”, configure the custom domain in repository **Settings** → **Pages**; the workflow must upload `CNAME` (and `.nojekyll`) into the artifact root.

## Troubleshooting

- [Troubleshooting custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages)
- Check DNS: `dig thevellalab.org +noall +answer -t A` should return the four GitHub A addresses from the doc above.
