# rewind-web

The public website for **Rewind**, an iOS app for reliving trips from your camera
roll. This repository holds the site only — the app itself is closed-source and
lives elsewhere.

**Live at:** https://nik-sloop.github.io/rewind-web/

| Page | URL | Why it exists |
| --- | --- | --- |
| Landing | [`/`](https://nik-sloop.github.io/rewind-web/) | App Store *Marketing URL* (optional) |
| Privacy Policy | [`/privacy.html`](https://nik-sloop.github.io/rewind-web/privacy.html) | App Store *Privacy Policy URL* — **required** |
| Terms of Use | [`/terms.html`](https://nik-sloop.github.io/rewind-web/terms.html) | EULA — **required**, because the app sells auto-renewing subscriptions |
| Support | [`/support.html`](https://nik-sloop.github.io/rewind-web/support.html) | App Store *Support URL* — **required** |
| Delete account | [`/delete-account.html`](https://nik-sloop.github.io/rewind-web/delete-account.html) | Linked from the privacy policy and the app |

## Why this is a separate repository

Because the app's repository is private, and GitHub Pages will not serve a
private repository on a free plan. Apple re-checks the privacy, terms and
support URLs on every submission, and a 404 on any of them is a rejection rather
than a warning — so the pages that must stay up live in a repository that can be
public, and nothing else does.

The cost of the split is that the app repository holds the canonical URLs in
`src/lib/links.ts` while the pages live here. If this repository is ever renamed
or moved to a bought domain, that constant has to change too, and so do the five
`<link rel="canonical">` tags in this repository.

## There is no build step

Hand-written HTML and one stylesheet. No npm, no generator, no bundler, no
dependencies at all. That is deliberate: these pages have to still be up in two
years, and the reliable way to achieve that is for there to be nothing that can
break. Edit the HTML directly.

To preview locally:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

Pages deploys automatically from the default branch on push — there is no
workflow to watch. `.nojekyll` is present so GitHub serves the files verbatim
instead of running Jekyll over them.

The colours in `styles.css` are copied from the app's `src/theme/colors.ts` and
the display font is the app's Space Grotesk, so the site and the app look like
one product. They are copied rather than shared because there is no build step
to share them through; if the app's palette changes, this is the second place to
change.

## Before this goes in front of a reviewer

The pages ship with deliberately loud placeholders — amber dashed boxes on the
page itself, so they are impossible to miss in a browser. All of them must be
filled in.

- [ ] Replace `support@example.com` with a mailbox that is actually read. **An
      App Store reviewer will email it, and a bounce is a rejection.** It
      appears 11 times across `privacy.html`, `terms.html`, `support.html` and
      `delete-account.html`.
- [ ] Replace `[YOUR FULL NAME]` with the trading name, in `privacy.html` and
      `terms.html`.
- [ ] Replace `[COUNTRY]` with the country of establishment.
- [ ] Set the governing law in `terms.html` §15 (`[COUNTRY / JURISDICTION]`) and
      the liability cap currency in §14 (`[CURRENCY]`).
- [ ] Name the real hosting region in the *International transfers* section of
      `privacy.html`. Being vague is fine; being wrong is not.
- [ ] Remove each `<div class="todo">` block once its contents are addressed.
- [ ] Confirm the App Store link on the landing page resolves. It points at app
      id `6799163619` and 404s until the first version is actually released.

Then paste the privacy, terms and support URLs into App Store Connect, and the
privacy and terms URLs into the app's paywall.

## Accuracy is the point

The privacy policy is not boilerplate. It was written against an audit of what
the app's code actually does, and it makes specific, checkable claims: that
photo-library scanning happens on-device, that only media added to a trip is
uploaded, that HealthKit is read for workouts and routes and nothing else, that
imported routes *are* uploaded, that there is no background location, and that
the app contains no analytics, advertising or crash-reporting SDK.

Apple cross-checks a privacy policy against the app's Privacy Nutrition Labels
and rejects mismatches. More to the point, a policy that overstates privacy is
worse than no policy at all. **If the app's data handling changes, change this
first.**

## Licence

The text and design of this site are &copy; its author and not offered for
reuse. It is public so that GitHub Pages can serve it, not as a template.
