# Launch follow-ups — audit, Bing, lead tracking, cleanups (2026-10-08)

Closes open items 1, 2, 4, 5 and 6 from `2026-10-07-seo-launch-hardening.md`. Run as three parallel agents
(audit, intake-tracking check, cleanups) plus the controller; nothing here touched Orbit.

## What was checked / done

**1. Production audit — all pass.** `python3 ~/dev/site-launch-kit/plugins/site-launch/scripts/sitekit_cli.py check-site overlandparkscollective.com`
(there is no `check-site` binary; that is the invocation). Home, sitemap, robots, favicon, llms.txt, og,
twitter, JSON-LD (Organization) and the GA tag `G-YSJR2FZ077` pass. Cross-checked by curl: the three
favicons and `og-image.jpg` 200; `/llms.txt` 200 as `text/plain; charset=utf-8` (the local `serve.mjs`
octet-stream concern does not apply in prod); JSON-LD parses on `/` (Organization) and the guide page
(WebPage); `https://www.` → apex 308 with path kept (`http://www.` takes two hops, harmless).

**2. Bing sitemap — Success.** Read via the kit's Bing API key (Keychain): `sitemap.xml` Status Success,
3 URLs, `IsVerified` true. Bing reports 959 bytes vs 458 served; URL count matches, so no action.

**3. Lead tracking.**
- Bird Dog Guide form: `generate_lead {form: bird_dog_guide}` verified reaching GA4 `/g/collect` from a
  real live submission (see `2026-10-07-bird-dog-guide-orbit-form.md`).
- Deal intake form: it is a cross-origin Orbit iframe whose submit is a Next.js Server Action, so a fake
  submit could not be stubbed safely and a real one would create a prod lead and email the team. Instead
  the agent posted the iframe's own success message (`orbit:submitted`) from inside the live iframe with
  every non-GET request to Orbit aborted: the page pushed `generate_lead {form: deal_intake}` and a
  `/g/collect` hit carried `en=generate_lead&ep.form=deal_intake`. **Not proven:** that a real submit emits
  `orbit:submitted` (read from Orbit's code, not run). Next real intake confirms it — check GA4 Realtime.
- **Gap found and fixed:** `generate_lead` was NOT a key event on the Overland property (557875397), so
  leads were not counting as conversions. Created via `sitekit_cli.py ga-key-event --property
  properties/557875397 --event generate_lead` → `created: true`.

**4. Cleanups.**
- `.gitignore` now ignores `.DS_Store`.
- JSON-LD `logo` now points at `brand_assets/overland_collective_logo-512.png` (512×512, 193 KB, made with
  `sips -Z 512`) instead of the 2000×2000 3 MB original, in `index.html`, `resources/index.html` and
  `resources/bird-dog-guide.html`. Original kept; og:image, favicons and on-page `<img>` logos untouched.

**5. GA account renamed** 410066150 "Goldsmith RV Park" → "Overland Parks Collective" via the kit's
gcloud ADC login (has `analytics.edit`); re-read confirms. Properties 556612296 (goldsmithrvpark.com) and
557875397 (Overland) unchanged; no IDs change. One account for all parks remains the design.

## Decisions + reasoning
- **No real intake submission for the tracking check.** It would write a lead to Orbit prod and notify the
  team; the website half (message → gtag → GA) is what this repo owns and is now proven.
- **Creating the key event without asking.** Marking `generate_lead` as a key event is the site-launch
  analytics standard and was the intent of the original launch; it changes reporting only, no data.
- **Logo as a `-512` sibling in `brand_assets/`**, matching the existing convention, not a root file; only
  JSON-LD uses it because crawlers fetch it and Google recommends ≥112px, so 512 is plenty.

## Useful context
- Kit quirk: `ga-key-event --property 557875397` returns an HTTP 404; it needs `properties/557875397`.
  Worth fixing in `site-launch-kit` (normalise the prefix).
- Agent scripts (Playwright, iframe message test) were scratch and not kept in the repo.

## Open
- Confirm in GA4 (Realtime or Reports → Key events) that the next real deal intake shows `generate_lead`.
- Check Ubersuggest after its first weekly refresh (log item 3, unchanged).
