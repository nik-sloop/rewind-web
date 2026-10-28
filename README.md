# The website

The source of the Rewind website, served by GitHub Pages from the public
`nik-sloop/rewind-web` repository at https://getrewind.cloud. It lives here, next to the app, because two of its
pages are the app's own words and must not drift from them; it is served
from there because this repository is private and Pages will not serve a
private repository on a free plan.

| File | Where it comes from |
| --- | --- |
| `privacy.html`, `terms.html` | **Generated.** `node scripts/build-legal-pages.mjs` writes them from `src/content/legal.ts`, the text the app shows on its own screens. Never edit the HTML. |
| `404.html` | **Generated** by the same script: the page a universal link lands on for somebody without the app, and the not-found page otherwise. |
| `index.html`, `support.html`, `delete-account.html` | Hand-written. Edit them directly. |
| `styles.css`, `icon.png`, `favicon.png` | The design, copied from `rewind-web`. The colours are the app's `src/theme`. |
| `.well-known/apple-app-site-association` | Tells iOS that `com.periq.app` may open `/trip/*` and `/world/*`. Only works at the root of a domain — see below. |
| `CNAME`, `.nojekyll` | The custom domain (getrewind.cloud), and "serve the files verbatim". |

## Publishing a change

1. Edit `src/content/legal.ts` (or a hand-written page) and run
   `node scripts/build-legal-pages.mjs`. Commit both.
2. Copy the contents of this folder over the root of a `rewind-web` checkout
   — everything, including `.nojekyll` and `.well-known/` — and push. Pages
   deploys on push; there is no workflow to watch.

Until the placeholders in `LEGAL` (`src/content/legal.ts`) are filled, the
two legal pages carry an amber warning box, on purpose: a reviewer opening a
policy that says "[YOUR FULL NAME]" is a rejection.

The App Store Connect URLs are `https://getrewind.cloud/privacy.html`,
`/terms.html` and `/support.html`.

## The domain, once

Universal links — a shared `https://` link opening the app directly — need
the association file at the root of the domain, and the shipping build
claims `getrewind.cloud` for them (`app.config.ts`). To wire the domain up:

1. At the registrar: four A records on the apex to `185.199.108.153`,
   `185.199.109.153`, `185.199.110.153` and `185.199.111.153`, and a CNAME
   for `www` to `nik-sloop.github.io`. Remove any parking records first.
2. In `rewind-web` → Settings → Pages, set the custom domain to
   `getrewind.cloud` and, once the DNS check passes, tick "Enforce HTTPS".
   `CNAME` in this folder is the same setting as a file, so a push keeps it.
3. Check `https://getrewind.cloud/.well-known/apple-app-site-association`
   returns the JSON directly, without a redirect. Pages serves it as
   `application/octet-stream`; Apple's crawler takes the file on its
   contents. If a build's device logs ever say the domain was not
   associated, the fallback is any host that sets `application/json` on
   that one path.
4. Set `EXPO_PUBLIC_WEB_ORIGIN=https://getrewind.cloud` in the EAS `preview`
   (and `production`) environment. That is the switch for `https://` share
   links (`src/lib/universalLinks.ts`): off, links stay as the `periq://`
   scheme; on, a shared link is `https://getrewind.cloud/trip/…`, which
   opens the app on a phone that has it and this site's 404 page — the
   fallback — for everybody else. Turn it on only after step 3, or a
   shared link would 404 for people without the app.

To serve from a different domain later, change the default in
`src/lib/legal.ts`, `app.config.ts` and `scripts/build-legal-pages.mjs`,
rebuild, replace `CNAME`, and swap the three hand-written pages' canonicals.

The App Store link on these pages is the record's id (`6799163619`); it
resolves once the app is live.
