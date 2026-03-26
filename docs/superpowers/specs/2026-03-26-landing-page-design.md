# Overland Collective — Landing Page Design Spec

## Context

Overland Collective needs a professional landing page to attract deal flow — sellers, bird dogs, and wholesalers who have RV parks or mobile home parks to sell. The page should establish brand credibility, clearly present acquisition criteria (buy boxes), and make it dead simple to submit a deal. Investors are served with a simple email link, not a dedicated section.

## Brand Foundation

**Colors:**
- Primary background: White (`#FFFFFF`)
- Text: Obsidian Black (`#1A1A1A`)
- Accent: Corten Rust (`#8B4513`)
- Secondary: Stone Grey (`#6C6C6C`)
- Alternating section backgrounds: White and light warm grey (`#F7F5F2`)

**Typography:**
- Headings: Montserrat Bold (Google Fonts), tight tracking (`-0.03em`)
- Body: Clean sans-serif (Inter or system-ui), generous line-height (`1.7`)
- Section labels: Montserrat, uppercase, small, letter-spacing `0.15em`, Corten Rust color

**Voice:** Direct, warm, assured. Not salesy. Not fluffy.

**Logo:** Circular compass seal — `brand_assets/overland_collective_logo.png`

## Tech Stack

- Single `index.html` file, all styles inline via Tailwind CSS CDN
- Mobile-first responsive
- Placeholder images via `https://placehold.co/WIDTHxHEIGHT`
- Real logo from `brand_assets/`
- GoHighLevel form: placeholder `<div>` with comment for future embed code

## Page Structure (Top to Bottom)

### 1. Navigation Bar
- **Position:** Sticky top, white background with subtle bottom border
- **Left:** OC logo (compass seal) + "OVERLAND COLLECTIVE" wordmark
- **Right:** Anchor links — About, Buy Boxes, Send a Deal, Investors
- **Mobile:** Hamburger menu
- **Behavior:** Smooth scroll to section anchors

### 2. Hero Section
- **Background:** Clean flat white
- **Content (centered):**
  - Label: "BUILT TO STAY. DRIVEN TO SERVE." (small, uppercase, Corten Rust)
  - Headline: "Got a Park? Let's Talk." (large, Montserrat Bold, Obsidian Black)
  - Subtext: "We acquire and operate RV parks and mobile home parks across the U.S. Check our buy boxes, then send us your deal." (Stone Grey, max-width ~500px)
  - Two CTAs side by side:
    - "View Buy Boxes" — filled Corten Rust button (scrolls to buy box section)
    - "Send a Deal" — outlined Corten Rust button (scrolls to form section)
- **No background image.** Clean and typographic.

### 3. About Section
- **Background:** Light warm grey (`#F7F5F2`)
- **Layout:** Two-column on desktop (text left, image right), stacks on mobile
- **Content:**
  - Label: "WHO WE ARE"
  - Headline: "We Don't Flip. We Hold."
  - Body copy: ~3 sentences about OC's long-term hold strategy, community investment, and operational approach. Weave in core values (Permanence, Unreasonable Hospitality, Grit & Grace) naturally — don't list them as bullet points.
  - Stats row below copy: 3 stat blocks — "5-Year Hold", "Nationwide", "Value-Add Focus" — with Corten Rust accent text
  - Right side: Placeholder image (park or team photo), rounded corners

### 4. Buy Boxes Section
- **Background:** White
- **Layout:** Centered header + two side-by-side cards
- **Header:**
  - Label: "WHAT WE BUY"
  - Headline: "Our Acquisition Criteria"
  - Subtext: "Check the boxes below for guidance, but don't hesitate to send deals that are close."
- **Cards (equal width, side by side on desktop, stacked on mobile):**
  - **Mobile Home Parks card:**
    - Icon/emoji + title
    - Criteria list pulled from buy box image:
      - 50+ pads, preference for TOH
      - $150k+ NOI or value-add opportunity meeting 50-site threshold
      - Within 45 min of growing metros
      - No waste treatment plants
      - Open to creative financing, seller financing, JVs
      - Cash flow and value-add focus, 5-year hold
  - **RV Parks card:**
    - Icon/emoji + title
    - Criteria list pulled from buy box image:
      - 50+ sites, open to mixed-use
      - $150k+ NOI or value-add opportunity meeting 50-site threshold
      - Established/underperforming parks (transient & long-term)
      - No flood zones, high crime, or declining markets
      - Strong amenity potential
      - Operational improvements, rent growth, amenity enhancements
  - Card styling: Light border, subtle shadow, rounded corners. Corten Rust accent on icon/header.

### 5. Send a Deal Section
- **Background:** Light warm grey (`#F7F5F2`)
- **Layout:** Centered
- **Content:**
  - Label: "SEND A DEAL"
  - Headline: "Have a Park That Fits?"
  - Subtext: "Whether you're a seller, broker, or wholesaler — we want to hear from you."
  - **GoHighLevel form placeholder:** A styled `<div>` with a dashed border and comment `<!-- Replace with GoHighLevel embed code -->`. Sized to roughly approximate a form (max-width ~500px, ~300px tall).

### 6. Investor Banner
- **Background:** Obsidian Black (`#1A1A1A`)
- **Layout:** Single centered strip, compact
- **Content:**
  - Headline: "Interested in Investing?" (white)
  - Subtext: "We're open to creative financing, seller financing, and joint ventures." (Stone Grey)
  - Email CTA: `money@overlandparkscollective.com` — styled as an outlined button or clickable `mailto:` link in Corten Rust

### 7. Footer
- **Background:** Obsidian Black (`#1A1A1A`)
- **Content (centered, small text):**
  - "© 2026 Overland Collective. Built to Stay."
  - "Acquiring & Operating RV Parks and Mobile Home Parks Across the United States"
  - Muted Stone Grey text

## Design Guardrails (from CLAUDE.md)

- No default Tailwind palette colors — only OC brand hex values
- No `shadow-md` — use layered, color-tinted shadows with low opacity
- No `transition-all` — animate only `transform` and `opacity`
- Every clickable element gets hover, focus-visible, and active states
- Headings and body use different font families
- Intentional, consistent spacing tokens
- Surface layering system (base → elevated)

## Responsive Behavior

- **Desktop (1024px+):** Two-column layouts for About and Buy Box cards, full nav links
- **Tablet (768px–1023px):** Cards may stay side-by-side at reduced width, nav collapses
- **Mobile (<768px):** All sections stack single-column, hamburger nav, full-width cards and form

## Verification Plan

1. Run `node serve.mjs` and open `http://localhost:3000`
2. Check all sections render correctly at desktop, tablet, and mobile widths
3. Verify smooth scroll navigation works for all anchor links
4. Verify `mailto:` link works on investor email
5. Confirm brand colors match spec (inspect hex values)
6. Confirm Montserrat loads from Google Fonts
7. Test hover/focus states on all buttons and nav links
8. Verify GoHighLevel placeholder div is clearly marked for future embed
