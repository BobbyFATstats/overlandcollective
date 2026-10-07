# 2026-10-07: Launch hardening for overlandparkscollective.com

This session ran the site-launch kit's read-only audit on this site, then closed every gap it found. Four lanes ran in parallel, and an independent read-only agent checked each one. Kit-side details (setup on this Mac and the kit fixes) are in `~/dev/site-launch-kit/HANDOFF.md`.

## Audit (before)

| Area | Found | Gap |
|---|---|---|
| Hosting / redirects | Home 200, sitemap and robots OK; `ovcoparks.com` and `www.ovcoparks.com` 308 to apex | `www.overlandparkscollective.com` served 200 (duplicate host) |
| Analytics | GA4 property 557875397, stream `G-YSJR2FZ077` on all pages | none |
| Onsite SEO | Title, description, canonical on all 3 pages | og/twitter tags, JSON-LD, favicon link; `/favicon.ico` and `/llms.txt` 404 |
| Search Console | Verified (siteOwner) | none |
| Bing | none | Site not in Bing Webmaster Tools |
| Ubersuggest | none | No project |
| Email DNS | MX, SPF, DKIM, DMARC (`p=quarantine`) all correct | none |

## Work completed

1. **www redirect (live).** Vercel project `overlandcollective` (`prj_miVXKDOcLAXKAV2Q8uRWA0LDIcNP`, team `bobbyfatstats-projects`). The project domain `www.overlandparkscollective.com` was changed from no redirect to `redirect: overlandparkscollective.com`, status 308, through the Vercel API. No DNS change: www is already a CNAME to the apex, and both resolve to Vercel. Paths and query strings are kept. Verified: www goes 308 to the apex and lands on a 200, with no loops, and the other four domains are unchanged.
2. **Onsite SEO (this commit).**
   - **Meta tags:** all 3 pages now have og:* and twitter:* tags (`summary_large_image`). og/twitter title and description mirror each page's existing `<title>` and meta description exactly, and `og:url` equals the canonical.
   - **JSON-LD:** home has an `Organization`; the two resource pages have `WebPage` with `publisher` pointing to that Organization.
   - **New files:** `favicon.ico` (16/32/48), `favicon-32x32.png` and `apple-touch-icon.png` (180), all built from `brand_assets/overland_collective_logo.png`. Also `og-image.jpg` (1200x630, cropped from `brand_assets/overland_collective_sign.png`) and `llms.txt`.
   - **`.gitignore`:** two exceptions, `!/apple-touch-icon.png` and `!/favicon-32x32.png`, because `*.png` is ignored and the icons would otherwise never deploy.
   - **Checks:** the kit's `seo-audit` is clean, and every new URL returned 200 on localhost.
3. **Bing Webmaster Tools.** Imported from Google Search Console, selecting only this site, under admin@overlandparkscollective.com. `IsVerified: true`. Sitemap `https://overlandparkscollective.com/sitemap.xml` submitted, status "Processing" on 2026-10-07.
4. **Ubersuggest project** "Overland Collective" created under admin@overlandparkscollective.com, with the sitemap attached.
   - **Competitors (10):** havenparkcommunities.com, horizonlandco.com, inspirecommunities.com, rhpproperties.com, flagshipcommunities.com, lakeshorecommunities.com, spaciousskiescampgrounds.com, rvparkstore.com, thecampgroundmarketplace.com, mobilehomeuniversity.com.
   - **AI prompts (15)** in 5 topics: Selling a Mobile Home Park, Selling an RV Park, Park Valuation for Sellers, Selling a Park with Seller Financing, and Bird Dogging Park Deals.
   - **Keywords (21):** seller and broker intent, e.g. "sell my mobile home park", "we buy rv parks", "mobile home park broker", "mobile home parks for sale by owner", plus the brand terms "overland collective" and "overland parks collective".

## Decisions and reasoning

- **Ubersuggest is set up from the seller's side, not the resident's.** The site exists to get deal flow from park owners, brokers and bird dogs. Ubersuggest's defaults (big REITs like Sun, ELS, UMH, YES, Hometown; keywords like "mobile home park near me" and "for rent") track residents and home shoppers, which would fill the reports with keywords you'll never target. Low-volume seller keywords (0-50 searches a month) are kept on purpose, because each searcher is a potential seller.
- **The AI prompts track whether ChatGPT and Perplexity name Overland when a seller asks who buys parks.** Ubersuggest's suggested prompts asked the question from an investor's side, which is your side, so they wouldn't measure that.
- **The JSON-LD is `Organization`, not `LocalBusiness`.** This site is a company acquiring parks nationwide, not a single physical location, so no address or phone was added. No `sameAs`, because the site links to no social profiles.
- **The Bird Dog Guide page is `WebPage`, not `Article`.** The page is a sign-up form; the guide itself is delivered by email.
- **Bing via the Search Console import.** It verifies instantly and needs no DNS record. The kit's API path verifies only through a DNS CNAME, and the GoDaddy token is read-only.
- **The www redirect was a Vercel project-domain setting.** No DNS or code change was needed. The before-state was a plain domain with no redirect; to undo, PATCH the www domain back to `redirect: null`.

## Useful context

- `brand_assets/overland_collective_logo.png` is 2000x2000 and about 3 MB. JSON-LD uses it as the logo. A 512px copy would be lighter for crawlers.
- The site has no `vercel.json`. Redirects and host settings live in the Vercel project's domain config.
- GA account 410066150 (currently named "Goldsmith RV Park") holds both the goldsmithrvpark (556612296) and Overland (557875397) properties. One account for all parks is the intended design.
- The GoDaddy API login (`gddy`) has read-only domain scope. DNS writes will need a broader login.
- Local `serve.mjs` serves `llms.txt` as `application/octet-stream`; confirm production serves `text/plain`.

## Open items / recommended next

1. Re-run the kit audit about a day after deploy to confirm production shows favicon, `llms.txt` and the og tags (`check-site overlandparkscollective.com`).
2. Check Bing Sitemaps in 1-2 days: the status should move from Processing to Success.
3. Check Ubersuggest after its first weekly refresh: site audit, rankings and AI visibility baseline.
4. Optional: a smaller logo for JSON-LD, and add `.DS_Store` to `.gitignore`.
5. Optional: rename the GA account "Goldsmith RV Park" to "Overland Parks Collective". This is cosmetic and changes no IDs.
6. The lead form isn't detected by the kit's static scan, because it's rendered by JavaScript. Confirm the GA "lead" key event fires on intake submit.
