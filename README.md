# SynthoCore BioLabs — Catalogue Site

A single self-contained `index.html` (no build step, no dependencies to install) — ready to drop into a GitHub repo and serve with GitHub Pages.

## Launch it on GitHub Pages

1. Create a new repository on GitHub (public repos get free Pages hosting).
2. Add `index.html` to the root of the repo (drag-and-drop on github.com works fine, or `git add`, `git commit`, `git push`).
3. In the repo: **Settings → Pages → Build and deployment → Source** → select **Deploy from a branch**.
4. Under **Branch**, choose `main` (or `master`) and folder `/ (root)`, then **Save**.
5. Wait ~1 minute, then refresh — GitHub shows your live URL at the top of the Pages settings, usually:
   `https://<your-username>.github.io/<repo-name>/`

## Using synthocore.com as the domain

A `CNAME` file (containing just `synthocore.com`) is included — put it in the repo root alongside `index.html`. That tells GitHub Pages which domain to serve.

Two things still need to happen for it to actually work:

1. **At your domain registrar** (wherever `synthocore.com` is registered), add these DNS records:
   - Four **A** records for the apex domain (`synthocore.com`), pointing to GitHub Pages' IPs:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - One **CNAME** record for `www` pointing to `<your-username>.github.io`.
     (If your registrar supports ALIAS/ANAME records instead of only A records for the apex, you can use one of those pointing at `<your-username>.github.io` instead of the four A records — check your provider's docs.)
   - DNS changes can take anywhere from a few minutes to 24 hours to propagate.

2. **In the repo**, go to **Settings → Pages** and enter `synthocore.com` under **Custom domain**, then save. GitHub will verify the DNS and can auto-provision an HTTPS certificate — once it shows as verified, tick **Enforce HTTPS**.

If you register the domain through a different registrar than where the site is hosted, the DNS records still go wherever the domain's nameservers point (usually the registrar itself, unless you've pointed it elsewhere).

## Managing products

The catalogue lives as a plain list near the bottom of `index.html`, inside the `<script>` block. Each product is one entry like this:

```js
{name:"BPC-157", alt:"Pentadecapeptide BPC 157", cas:"137525-51-0", mw:"1419.53", purity:"99.1%", size:"5 mg", cat:"repair"}
```

To add, remove, or change a product, edit that list directly, then re-upload `index.html` to GitHub — GitHub Pages picks up the change automatically once it deploys (usually under a minute). `cat` should be one of `repair`, `metabolic`, `growth`, `cosmetic` (these match the filter buttons on the site — add more buttons in the HTML if you add categories).

There are no prices or stock levels shown. A banner reading "Submit a research enquiry via the contact form" sits above the catalogue table, and each row still has a Request button that jumps down to the contact form and pre-fills the compound name.

## Before you actually launch

The catalogue and form are wired up with placeholder content you'll want to swap out:

- **Products** — edit the `products` list near the bottom of `index.html` (see the "Managing products" section above).
- **Contact details** — replace the phone placeholder in the "Request a quote" section and footer. The email is already set to `synthocorebiolabs@gmail.com`.
- **The inquiry form is wired up and live** via [FormSubmit](https://formsubmit.co) — no backend, no signup required. **Important: the very first submission** (send yourself a test one after launch) will land as a "please confirm" email in `synthocorebiolabs@gmail.com`'s inbox instead of the actual message — click the confirmation link in it once, and every submission after that arrives normally as an email. Check spam/junk if you don't see it.
  - The form uses FormSubmit's AJAX endpoint, so visitors see an inline "thanks" message instead of being redirected away.
  - A hidden honeypot field (`_honey`) cuts down on spam bots, and FormSubmit's captcha step is disabled since the honeypot plus AJAX already filters most of it.
  - Because this method needs no private API key, the destination email address is visible in the page's HTML source. If you'd rather keep it out of public view later, swap to a service like [Formspree](https://formspree.io), which uses a private form ID instead.
- **Favicon / social preview** — none is set; add a `<link rel="icon">` and Open Graph tags in `<head>` if you want one.
- **Legal/compliance copy** in the footer is a starting point, not legal advice — have it reviewed for your actual jurisdiction and product line.

## Structure

Everything — HTML, CSS, and JS — lives in the one `index.html` file, so there's nothing else to configure. If the catalogue grows large, consider splitting the `products` array into a `products.json` file and fetching it, but for a few dozen items the inline array is simpler and loads instantly.
