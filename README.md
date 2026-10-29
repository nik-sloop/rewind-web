# The Rewind website

Served by GitHub Pages at https://getrewind.cloud, straight from `main`.
Pages deploys on push; there is no workflow to watch.

| File | Where it comes from |
| --- | --- |
| `privacy.html`, `terms.html` | **Generated** from the app's own words. In the app repository (`nik-sloop/periq`), `node scripts/build-legal-pages.mjs ../rewind-web` writes them here from `src/content/legal.ts`, the text the app shows on its own screens, so the app and the site can never say different things. Never edit the HTML; edit `legal.ts`, run the script, commit both repositories. |
| `404.html` | **Generated** by the same script: the page a universal link lands on for somebody without the app, and the not-found page otherwise. |
| `index.html`, `support.html`, `delete-account.html` | Hand-written. Edit them here. |
| `styles.css`, `icon.png`, `favicon.png` | The design. The colours are the app's `src/theme`. |
| `.well-known/apple-app-site-association` | Tells iOS that `com.periq.app` may open `/trip/*` and `/world/*`. Only works at the root of a domain. |
| `CNAME`, `.nojekyll` | The custom domain (getrewind.cloud), and "serve the files verbatim". |

The App Store Connect URLs are `https://getrewind.cloud/privacy.html`,
`/terms.html` and `/support.html`. Support is rewind.support@gmail.com,
written in the hand-written pages here and in `legal.ts` there; change both.

## The domain

Universal links — a shared `https://` link opening the app directly — need
the association file at the root of the domain, and the shipping build
claims `getrewind.cloud` for them (`app.config.ts` in the app repository).

1. At the registrar: four A records on the apex to `185.199.108.153`,
   `185.199.109.153`, `185.199.110.153` and `185.199.111.153`, and a CNAME
   for `www` to `nik-sloop.github.io`. No other records on the apex.
2. Settings → Pages: custom domain `getrewind.cloud`; once the DNS check
   passes and the certificate is issued, tick "Enforce HTTPS". `CNAME` here
   is the same setting as a file, so a push keeps it.
3. Check `https://getrewind.cloud/.well-known/apple-app-site-association`
   returns the JSON directly, without a redirect. Pages serves it as
   `application/octet-stream`; Apple's crawler takes the file on its
   contents. If a build's device logs ever say the domain was not
   associated, the fallback is any host that sets `application/json` on
   that one path.
4. In the app's EAS `preview` and `production` environments, set
   `EXPO_PUBLIC_WEB_ORIGIN=https://getrewind.cloud`. That is the switch for
   `https://` share links: off, links stay as the `periq://` scheme; on, a
   shared link is `https://getrewind.cloud/trip/…`, which opens the app on
   a phone that has it and this site's 404 page for everybody else. Turn it
   on only after step 3, or a shared link would 404 for people without the
   app.

To serve from a different domain later, change `CNAME` here and the three
hand-written pages' canonicals, and in the app repository the defaults in
`src/lib/legal.ts`, `app.config.ts` and `scripts/build-legal-pages.mjs`,
then rebuild.

The App Store link on these pages is the record's id (`6799163619`); it
resolves once the app is live.
