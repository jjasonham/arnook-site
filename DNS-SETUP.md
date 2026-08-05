# Pointing arnook.app to GitHub Pages (Namecheap)

This guide gets your GitHub Pages site live at **https://arnook.app**. Follow it
top to bottom. You do this once. DNS changes can take up to 24 hours to spread
across the internet, but it is usually much faster (often under an hour).

---

## Before you start

- You need to be logged in to **Namecheap**, where arnook.app is registered.
- You need the GitHub repository that holds this site, and it needs GitHub Pages
  turned on (see the last section).
- The GitHub username that owns the repo is likely **jjasonham** — please confirm
  this before you use it below. Wherever you see `<YOUR-GITHUB-USERNAME>`, swap in
  the confirmed username.

---

## Step 1 — Open the DNS editor in Namecheap

1. Log in to Namecheap.
2. Go to **Domain List** in the left menu.
3. Find **arnook.app** and click **Manage**.
4. Click the **Advanced DNS** tab.

You will see a table called **Host Records**.

---

## Step 2 — Remove the parking / redirect records first (important)

The domain currently shows a Namecheap parking page. Those parking records will
fight with GitHub Pages, so they must go first.

- Delete any record whose **Type** is **URL Redirect Record** or **URL Frame**.
- Delete any parking-related **CNAME Record** on the `@` host or the `www` host.
- Also delete any existing **A Record** on `@` that points somewhere else.

> In Namecheap, the apex domain (arnook.app with nothing in front) is written as
> `@` in the **Host** column. That is normal.

Leave any email-related records (MX, TXT for email) alone unless you know they are
unused.

---

## Step 3 — Add the four A records for the apex (`@`)

Click **Add New Record** and add each of these. Type = **A Record**, Host = `@`,
TTL = **Automatic**.

| Type     | Host | Value             |
|----------|------|-------------------|
| A Record | @    | 185.199.108.153   |
| A Record | @    | 185.199.109.153   |
| A Record | @    | 185.199.110.153   |
| A Record | @    | 185.199.111.153   |

All four. They point the bare domain at GitHub's servers.

---

## Step 4 — Add the four AAAA records for the apex (`@`)

These are the IPv6 versions. Type = **AAAA Record**, Host = `@`, TTL =
**Automatic**.

| Type        | Host | Value                   |
|-------------|------|-------------------------|
| AAAA Record | @    | 2606:50c0:8000::153     |
| AAAA Record | @    | 2606:50c0:8001::153     |
| AAAA Record | @    | 2606:50c0:8002::153     |
| AAAA Record | @    | 2606:50c0:8003::153     |

---

## Step 5 — Add one CNAME record for `www`

This makes **www.arnook.app** work too, redirecting to GitHub.

Type = **CNAME Record**, Host = `www`, TTL = **Automatic**.

| Type         | Host | Value                             |
|--------------|------|-----------------------------------|
| CNAME Record | www  | `<YOUR-GITHUB-USERNAME>.github.io` |

Example, if the confirmed username is `jjasonham`, the Value would be
`jjasonham.github.io`. Do **not** put `https://` or a slash in the value — just the
host name, and Namecheap may add a trailing dot automatically. That is fine.

---

## Step 6 — Save

Click the green **Save All Changes** check mark. Your Host Records table should now
show: four A records on `@`, four AAAA records on `@`, and one CNAME on `www`.

---

## Step 7 — Set the custom domain on GitHub

1. Go to your GitHub repository for this site.
2. Click **Settings** → **Pages**.
3. Under **Custom domain**, type `arnook.app` and click **Save**.
   - The repo already includes a `CNAME` file with `arnook.app` in it, so GitHub
     may fill this in for you.
4. Wait for DNS to propagate (up to 24 hours, usually much less). GitHub will run a
   DNS check.
5. Once the check passes, tick **Enforce HTTPS**. This gives you the secure
   `https://` padlock that Apple requires for the privacy and support pages.

---

## How to know it worked

- Visiting **https://arnook.app** shows the Arnook landing page.
- **https://arnook.app/privacy.html** shows the privacy policy.
- **https://arnook.app/support.html** shows the support page.
- The browser shows a padlock (HTTPS) with no warning.

If it still shows the parking page after a few hours, re-check Step 2 — a leftover
URL Redirect record is the most common cause.
