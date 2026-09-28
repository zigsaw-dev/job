# Zigsaw links on your own domain (jobs.zigsaw.in)

**Why:** on phones, `script.google.com` links fail when the browser is signed in to several Google accounts, or when Android opens them in the Google Drive app ("Sorry, unable to open the file"). Incognito works because it has no Google sign-in. The fix is a tiny page on your own domain that shows the app inside it. Every e-mail and share link then becomes `https://jobs.zigsaw.in/?page=…`.

**Files:**
- `index.html`: the page. Its only setting is `EXEC` near the bottom, your live /exec URL.
- `404.html`: an identical copy, so any path on jobs.zigsaw.in still opens the app.
- `CNAME`: the domain name, for GitHub Pages.

## Step by step (GitHub Pages, free, about 20 minutes)

1. **Check `EXEC`** in `index.html` (and `404.html`). It must be your live deployment in the public form `https://script.google.com/macros/s/<id>/exec`, without `/a/zigsaw.in/`.
2. **Create a GitHub repository.**
   - Go to github.com → New repository → name it `zigsaw-jobs`.
   - Make it **Public**. Pages on a free account needs a public repo. The page contains nothing secret; the /exec URL is public anyway.
   - Then choose Add file → Upload files, drop in `index.html`, `404.html` and `CNAME`, and click Commit.
3. **Turn on Pages.** In the repository go to Settings → Pages. Under *Build and deployment*, choose Source **Deploy from a branch**, Branch **main**, folder **/(root)**, then Save.
4. **Add the DNS record.** At the company that manages DNS for **zigsaw.in** (where you bought the domain, e.g. Squarespace/Google Domains, GoDaddy or Cloudflare), add:

   | Type | Name / Host | Value / Points to | TTL |
   |---|---|---|---|
   | CNAME | `jobs` | `<your-github-username>.github.io` | default |

   Don't touch the existing MX and other records: Gmail keeps working.
5. **Connect the domain.**
   - Back in Settings → Pages, set **Custom domain** to `jobs.zigsaw.in`, then Save.
   - Wait for the green *DNS check successful*. It usually takes minutes and can take up to a few hours.
   - Then tick **Enforce HTTPS**. It becomes available once the certificate is issued.
6. **Test it on your phone,** in the normal browser rather than incognito:
   - `https://jobs.zigsaw.in/?page=health` should show `ok`.
   - `https://jobs.zigsaw.in/?page=job&id=J-8A72DD3A6541` should open the job.
   - Also send yourself a link in WhatsApp and Gmail and tap it there.
7. **Switch the app's links.** In Admin → Settings, set `WEB_APP_URL` = `https://jobs.zigsaw.in`. From then on, all e-mails (invitations, résumé links, claim / set-password, welcome, remove) and the "Share this job" links use it. Run *Run diagnostics*; the line should read `OK  E-mail links use: https://jobs.zigsaw.in`.

## Later: when you deploy a new version

Keep using Deploy → Manage deployments → ✏️ → **New version**. The /exec URL stays the same, so nothing here changes. Edit `EXEC` in both HTML files only if you ever create a new deployment.

## Worth knowing
- **Old links:** e-mails already sent keep their `script.google.com` links. Those still work on laptops and in incognito.
- **Page address:** the address bar stays on the address that was first opened while people move around inside the app. Refreshing returns to that starting page.
- **Signing in on iPhone:** a signed-in employer or staff member on iPhone Safari may have to sign in again after closing the tab. Browsers limit what embedded pages remember. Job, apply and résumé-download links are not affected.
- **Rollback:** set `WEB_APP_URL` back to the /exec URL.
