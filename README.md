# home

Typednotes homepage: a single self-contained [`index.html`](https://github.com/typednotes/home/blob/main/index.html) (no build step) rendering an
animated ASCII-art 3D node network with a "Typednotes" title overlay, plus two self-contained
legal pages: [`privacy.html`](https://github.com/typednotes/home/blob/main/privacy.html) (https://www.typednotes.com/privacy, the privacy policy URL for the
Google OAuth consent screen) and [`terms.html`](https://github.com/typednotes/home/blob/main/terms.html) (https://www.typednotes.com/terms).

It is published to GitHub Pages by the workflow in [`.github/workflows/pages.yml`](https://github.com/typednotes/home/blob/main/.github/workflows/pages.yml) on every
push to `main` (or manually via *Actions → Deploy to GitHub Pages → Run workflow*).

## Local preview

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## GitHub Pages setup (one time)

1. Repo **Settings → Pages → Build and deployment → Source**: select **GitHub Actions**.
2. Push to `main` (or run the workflow manually). The site is then available at
   `https://typednotes.github.io/home/`.
3. **Settings → Pages → Custom domain**: enter `www.typednotes.com` and save.
   GitHub checks the DNS records (see below). Once the check passes and the certificate is
   issued (can take up to ~1 hour), tick **Enforce HTTPS**.

Note: with an Actions-based deployment, a `CNAME` file in the repo is ignored; the custom
domain is configured in the repository settings only.

Equivalent with the GitHub CLI:

```sh
gh api -X POST repos/typednotes/home/pages -f build_type=workflow
gh api -X PUT  repos/typednotes/home/pages -f cname=www.typednotes.com
gh api -X PUT  repos/typednotes/home/pages -F https_enforced=true   # once the cert is issued
```

## DNS records

Configure these at the DNS provider of `typednotes.com`.

### `www` subdomain (the canonical address)

| Type  | Name  | Value                  |
|-------|-------|------------------------|
| CNAME | `www` | `typednotes.github.io` |

The CNAME points to the **organization's** Pages host (`<owner>.github.io`), without the
repository name.

### Apex domain `typednotes.com` (recommended)

Pointing the apex at GitHub too makes `typednotes.com` automatically redirect to
`www.typednotes.com`.

| Type | Name | Value                  |
|------|------|------------------------|
| A    | `@`  | `185.199.108.153`      |
| A    | `@`  | `185.199.109.153`      |
| A    | `@`  | `185.199.110.153`      |
| A    | `@`  | `185.199.111.153`      |
| AAAA | `@`  | `2606:50c0:8000::153`  |
| AAAA | `@`  | `2606:50c0:8001::153`  |
| AAAA | `@`  | `2606:50c0:8002::153`  |
| AAAA | `@`  | `2606:50c0:8003::153`  |

(If your provider supports `ALIAS`/`ANAME`/CNAME-flattening at the apex, you can instead
point `@` to `typednotes.github.io`.)

Remove any other conflicting `A`/`AAAA`/`CNAME` records for `@` and `www`, and do **not**
use wildcard records (`*.typednotes.com`) — they allow subdomain takeover.

### Domain verification (recommended)

Verifying the domain prevents other GitHub users from claiming it:

1. GitHub **organization settings → Pages → Add a domain** → `typednotes.com`.
2. Add the `TXT` record GitHub shows, of the form:

   | Type | Name                                           | Value                 |
   |------|------------------------------------------------|-----------------------|
   | TXT  | `_github-pages-challenge-typednotes`           | *(code from GitHub)*  |

3. Click **Verify**.

### Check

```sh
dig www.typednotes.com +noall +answer   # -> CNAME typednotes.github.io.
dig typednotes.com     +noall +answer   # -> the four 185.199.10x.153 A records
curl -sI https://www.typednotes.com | head -1
```

DNS changes can take up to 24 hours to propagate.
