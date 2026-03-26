# Publish vellalab.org from GitHub

This site is static HTML/CSS. Use **GitHub Pages** with the custom domain **vellalab.org**.

## Files in this repository

| File | Purpose |
|------|--------|
| `CNAME` | Tells GitHub Pages the hostname is `vellalab.org`. Must stay in the **published root**. |
| `.nojekyll` | Disables Jekyll so GitHub serves files as-is. |
| `dns/github-pages-vellalab.org.txt` | Copy-paste reference for DNS values. **Not** deployed with the site. |

DNS is configured at **GoDaddy** (or whatever hosts DNS for `vellalab.org`), not inside GitHub beyond the `CNAME` file.

---

## GoDaddy: point vellalab.org at GitHub Pages (“propagate” DNS)

**Propagation** means: after you save records in GoDaddy, resolvers worldwide pick them up (often under an hour, sometimes up to 24–48 hours). You don’t “submit” DNS to GitHub—GitHub only checks that lookups for `vellalab.org` return the right answers.

1. Sign in at [godaddy.com](https://www.godaddy.com) → **My Products** → **Domains** → **vellalab.org** → **DNS** or **Manage DNS**.

2. **Apex domain (`vellalab.org`):**  
   - Delete **A** records on **`@`** that point to GoDaddy parking or old hosts (if any).  
   - Add **four** **A** records:
     - **Type:** A  
     - **Name:** `@` (or “Domain” / blank—GoDaddy’s label for the root)  
     - **Value:** `185.199.108.153`  
   - Repeat for **`185.199.109.153`**, **`185.199.110.153`**, **`185.199.111.153`**.  
   - (Optional) Add **AAAA** records for `@` using the IPv6 addresses in `dns/github-pages-vellalab.org.txt`.

3. **`www.vellalab.org`:**  
   - Add **CNAME**: **Name** `www`, **Value** your GitHub Pages host, e.g. `vellalab.github.io` (replace with your **user or org** `*.github.io`—no `https://`, no `/repo`).  
   - If an old `www` record exists, edit or remove it so only this CNAME remains for `www`.

4. Save. Wait for propagation, then on GitHub: **Settings** → **Pages** → **Custom domain** = `vellalab.org` → Save. When the check passes, enable **Enforce HTTPS**.

---

## GitHub (summary)

1. **Settings** → **Pages** → **Source:** deploy from branch (e.g. `main`) / **`/` (root)**.  
2. **Custom domain:** `vellalab.org` (must match the `CNAME` file in the repo).  
3. After DNS works, **Enforce HTTPS**.

Optional: [Verify your custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages).

---

## Ongoing maintenance

- Push HTML/CSS changes to the Pages branch; keep **`CNAME`** as:

  ```text
  vellalab.org
  ```

---

## Troubleshooting

- [GitHub: troubleshooting custom domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages)  
- Terminal check: `dig vellalab.org +noall +answer -t A` should list the four GitHub **A** addresses.
