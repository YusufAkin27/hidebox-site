# HideBox public site

Static pages for the App Store / Google Play listings: support page, privacy
policy and terms of use, in English and Turkish. No build step, no JavaScript,
no external requests (system fonts, local CSS), only relative links, so it works
from any base path.

```
index.html            support page (EN)       tr/index.html
privacy/index.html    privacy policy (EN)     privacy/tr/index.html
terms/index.html      terms of use (EN)       terms/tr/index.html
assets/               style.css and the brand mark SVGs
.nojekyll             tells GitHub Pages to serve the folder as-is
```

## Placeholders to replace before publishing

Search and replace in every `.html` file (12 occurrences or more per language):

| Token | Meaning |
|---|---|
| `{{CONTACT_EMAIL}}` | public support / privacy contact address |
| `{{DEVELOPER_NAME}}` | developer or company name |
| `{{EFFECTIVE_DATE}}` | date the policy and terms take effect, e.g. `2026-11-11` |

For example: `grep -rl '{{CONTACT_EMAIL}}' . | xargs sed -i 's/{{CONTACT_EMAIL}}/you@example.com/g'`.
`https://yusufakin27.github.io/hidebox-site` below is only used in this README: it is the final address
of the site, with no trailing slash.

## Publish with GitHub Pages

The app repository is private, so publish from a small separate public repo.

**Option A: default branch at the root.** Create a public repo, copy the
contents of this folder (including the hidden `.nojekyll`) to the repo root on
the default branch, push. In the repo go to Settings > Pages, set Source to
"Deploy from a branch", Branch = default branch, folder = `/ (root)`.

**Option B: `gh-pages` branch.** In a public repo, push the contents of this
folder to the root of a branch named `gh-pages` (for example
`git subtree push --prefix site origin gh-pages` from a checkout that contains
this folder, or a plain commit on that branch). Settings > Pages: Source =
"Deploy from a branch", Branch = `gh-pages`, folder = `/ (root)`.

The site is then at `https://<user>.github.io/<repo>/` (a project site), or at
your custom domain if you add one under Settings > Pages. All links are
relative, so either works.

## URLs to enter in the stores

| Where | Field | URL |
|---|---|---|
| App Store Connect > App Information | Privacy Policy URL | `https://yusufakin27.github.io/hidebox-site/privacy/` |
| App Store Connect > App Store (version page) | Support URL | `https://yusufakin27.github.io/hidebox-site/` |
| Play Console > App content > Privacy policy | Privacy policy | `https://yusufakin27.github.io/hidebox-site/privacy/` |
| Play Console > Store settings (optional) | Website | `https://yusufakin27.github.io/hidebox-site/` |

Turkish versions: `https://yusufakin27.github.io/hidebox-site/privacy/tr/`, `https://yusufakin27.github.io/hidebox-site/tr/`.
Enter the same privacy URL in both stores and in the app
(`kPrivacyPolicyUri` in `lib/features/paywall/paywall_screen.dart`, currently
`https://hidebox.app/privacy`; it must point at the published page).
