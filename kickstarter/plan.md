# Lines in Art — Kickstarter Landing Page
## `plan.md` · For use with Claude Code

---

## 1. Goal

A single-page bridge site that converts warm traffic (friends, family,
Instagram followers, grandparents, email list, Italian contacts) into
Kickstarter backers — by showing the book clearly, presenting three
reward options with direct deep links, and completely removing the
anxiety caused by Kickstarter's login wall and unfamiliar pledge flow.

**The one thing a visitor must do:** click a reward button and land
on the correct Kickstarter reward pre-selected, ready to back.

**Success metric:** visitor understands what the book is, picks a
reward, and clicks through in under 60 seconds — even if they have
never used Kickstarter before and are not tech-savvy.

---

## 2. Audience

- **Primary:** parents of children ages 3–8, arriving from Instagram,
  WhatsApp shares, or a personal recommendation.
- **Secondary:** grandparents — assume minimal tech comfort. The page
  must be scannable by someone who struggles with online shopping.
  Large text, simple language, nothing that looks confusing or scammy.
- **Tertiary:** educators and art teachers arriving from email or Reddit.
- **Italian-speaking visitors:** a segment of the audience is Italian
  (family connections, Italian press). The page auto-detects the
  browser language and shows Italian copy if `navigator.language`
  starts with "it". A manual language toggle is also visible at
  the top of the page.

**No Kickstarter experience assumed for any visitor.** The page
must explain what Kickstarter is and what "pledging" means before
asking anyone to click a reward button.

---

## 3. Brand identity

### Official color palette (confirmed from brand sheet)

```css
--lia-blue:    #4356A6;  /* Ocean Twilight — primary, dominant */
--lia-orange:  #FF8F00;  /* Deep Saffron — strong accent */
--lia-yellow:  #FFBD05;  /* Amber Gold — warmth, highlights */
--lia-green:   #568203;  /* Forest Moss — nature, used sparingly */

/* Derived supporting colors */
--lia-white:   #FDFAF4;  /* warm off-white — page background */
--lia-cream:   #FDF6E3;  /* pale amber — section alternation */
--lia-text:    #1A1A2E;  /* near-black for body text */
--lia-muted:   #5A5F7A;  /* secondary / caption text */
```

**Color usage rules:**
- `--lia-blue` (#4356A6): hero elements, section backgrounds (one
  section only), primary CTA buttons, step numbers, borders
- `--lia-orange` (#FF8F00): scarcity badges, urgency labels,
  "Most popular" accents, discount callouts — use sparingly
- `--lia-yellow` (#FFBD05): accents on dark blue backgrounds
  (step number rings, hero highlights), decorative elements
- `--lia-green` (#568203): checkmarks in reward lists, "included"
  indicators — maps to "yes / good"
- Never use all four brand colors at equal visual weight.
  Blue dominates, orange punctuates, yellow warms, green confirms.

### Typography

- **Display / headings:** `Fraunces` (Google Fonts) — warm, slightly
  quirky optical-size serif. Echoes the hand-lettered feel of the
  book cover. Use italic variant for warmth.
  Import: `family=Fraunces:ital,opsz,wght@0,9..144,400..700;1,9..144,400..700`
- **Body / UI:** `Instrument Sans` (Google Fonts) — clean humanist
  sans, very readable at small sizes for grandparents.
  Import: `family=Instrument+Sans:wght@400;500`
- **Weights in use:** 400 regular, 500 medium, 700 bold only.
- **Body text minimum:** 16px — never smaller. Grandparent-friendly.
- **Line height:** 1.7 for body, 1.15 for headings.

### Aesthetic direction

"A beautifully printed children's art book, not a website."

Think: how the MoMA store or Tate Modern family program presents
books. Warm, trustworthy, slightly handcrafted. The cream background
with the navy book cover should feel like a book on a linen table
in a good museum café.

**Specific rules:**
- The book cover image does all the visual heavy lifting — don't
  compete with it.
- Generous whitespace — no crowding, no information density.
- Section alternation: warm cream (#FDF6E3) and white (#FDFAF4).
  One section uses brand blue (#4356A6) as background — the
  "How Kickstarter works" section only.
- Thin horizontal rules (1px, 15% opacity) as dividers. No thick
  decorative borders.
- Orange is loud — use only for scarcity/urgency signals like
  "Only a few left at this price." Not for decoration.

**What to explicitly avoid:**
- Inter, Roboto, system-ui, Arial
- Purple gradients, mesh backgrounds, glow effects
- SaaS product card patterns or feature comparison tables
- Dark hero sections with glowing text
- Floating 3D book mockups
- Emoji used as bullet points or UI icons
- Anything that makes this look like a tech product or app
- Accordion FAQ — grandparents won't click it; show everything open

---

## 4. Bilingual implementation (EN / IT)

### Detection logic (JS at page load)
```js
const saved = localStorage.getItem('lia-lang');
const browser = navigator.language?.startsWith('it') ? 'it' : 'en';
const lang = saved || browser;
setLanguage(lang);
```

### Toggle UI
Top-right corner of the page, always visible even when scrolled.
Simple text: `EN | IT` — active language is bold and underlined.
Clicking switches language, saves to localStorage.

### Implementation pattern
Every user-visible text element has `data-en` and `data-it`
attributes. A `setLanguage(lang)` function iterates all elements
with `[data-en]` and sets `textContent = el.dataset[lang]`.

```html
<p data-en="A children's picture book for looking at real masterpieces"
   data-it="Un libro illustrato per bambini per scoprire i grandi capolavori"></p>
```

The `<html lang="">` attribute is also updated on each switch.

### Italian copy for all key strings

```
Page <title>:
  EN: Lines in Art — Back us on Kickstarter
  IT: Lines in Art — Sostienici su Kickstarter

Hero eyebrow:
  EN: Now live on Kickstarter
  IT: Ora su Kickstarter

Hero H1 subhead:
  EN: A children's picture book for looking at real masterpieces
  IT: Un libro illustrato per bambini per scoprire i grandi capolavori

Hero description:
  EN: A playful way to slow down, notice details, and connect with
      your children through art — for ages 3 to 8.
  IT: Un modo giocoso per rallentare, osservare i dettagli e
      connettersi con i propri figli attraverso l'arte —
      per bambini dai 3 agli 8 anni.

Hero primary CTA:
  EN: See the rewards ↓
  IT: Scopri i premi ↓

Hero secondary link:
  EN: View the full Kickstarter campaign →
  IT: Vai alla campagna Kickstarter →

Book ages/format line:
  EN: Ages 3–8 · Hardcover · Ships [SHIP_DATE]
  IT: Età 3–8 anni · Copertina rigida · Spedizione [SHIP_DATE]

Section label — book preview:
  EN: Inside the book
  IT: Dentro il libro

Section heading — book preview:
  EN: Every spread is a conversation starter
  IT: Ogni pagina è un invito alla conversazione

Spread intro paragraph:
  EN: Each artist gets their own spread — a real masterpiece on the
      left, open questions and an activity on the right. Your child
      leads the looking.
  IT: Ogni artista ha la sua doppia pagina — un capolavoro autentico
      a sinistra, domande aperte e un'attività a destra. È il tuo
      bambino a guidare l'osservazione.

Mondrian caption:
  EN: Kids trace the lines with their finger, then hunt for the same
      grid in windows, buildings, and streets. Mondrian's world is
      everywhere once you know how to see it.
  IT: I bambini seguono le linee con il dito, poi le cercano nelle
      finestre, negli edifici e nelle strade. Il mondo di Mondrian
      è ovunque, basta saperlo guardare.

Van Gogh caption:
  EN: The swirling lines of the Starry Night become a finger-tracing
      adventure. Children discover how a line can carry emotion —
      calm, wild, or somewhere in between.
  IT: Le spirali della Notte Stellata diventano un percorso da
      seguire con il dito. I bambini scoprono come una linea possa
      portare un'emozione — calma, selvaggia, o qualcosa nel mezzo.

How it works — section label:
  EN: New to Kickstarter?
  IT: Non conosci Kickstarter?

How it works — heading:
  EN: Here's exactly how it works
  IT: Ecco come funziona, passo per passo

Step 1 title:
  EN: Choose a reward below
  IT: Scegli il tuo premio

Step 1 body:
  EN: Pick the option that's right for you and click the button.
      You'll go directly to that reward on Kickstarter.
  IT: Scegli l'opzione più adatta a te e clicca il pulsante.
      Arriverai direttamente a quel premio su Kickstarter.

Step 2 title:
  EN: Create a free account (60 seconds)
  IT: Crea un account gratuito (60 secondi)

Step 2 body:
  EN: Kickstarter will ask you to sign in. It's completely free.
      Your card is NOT charged until the campaign ends — and only
      if we reach our goal.
  IT: Kickstarter ti chiederà di accedere. È completamente gratuito.
      La tua carta NON viene addebitata fino alla fine della campagna
      — e solo se raggiungiamo il nostro obiettivo.

Step 3 title:
  EN: We print and ship to you
  IT: Stampiamo e ti spediamo il libro

Step 3 body:
  EN: Once funded, we go to print. Books ship in [SHIP_DATE].
      You'll receive email updates every step of the way.
  IT: Una volta finanziati, andiamo in stampa. I libri vengono spediti
      a [SHIP_DATE]. Riceverai aggiornamenti via email.

Reassurance below steps:
  EN: You can cancel your pledge at any time before the campaign
      closes. If we don't reach our goal, you owe nothing.
  IT: Puoi annullare il tuo sostegno in qualsiasi momento prima della
      fine della campagna. Se non raggiungiamo l'obiettivo, non ti
      verrà addebitato nulla.

Rewards section label:
  EN: Choose your reward
  IT: Scegli il tuo premio

Rewards heading:
  EN: Three options. All Kickstarter-exclusive pricing.
  IT: Tre opzioni. Prezzi esclusivi per la campagna.

Rewards subtext:
  EN: These links take you directly to the discounted price.
      More options are available on the full Kickstarter page.
  IT: Questi link portano direttamente al prezzo scontato.
      Altre opzioni sono disponibili sulla pagina Kickstarter.

Most popular badge:
  EN: Most popular
  IT: Il più scelto

Scarcity label:
  EN: Only a few left at this price
  IT: Ultimi disponibili a questo prezzo

Discount badges:
  EN: 35% off / 10% off / 34% off
  IT: 35% di sconto / 10% di sconto / 34% di sconto

Tier 1 name / tagline:
  EN: One Book / A single signed hardcover
  IT: Un libro / Un'unica copia firmata con copertina rigida

Tier 2 name / tagline:
  EN: Three Books / Best value — perfect for gifting
  IT: Tre libri / Il miglior rapporto qualità-prezzo — ideale come regalo

Tier 3 name / tagline:
  EN: Five Books / For classrooms or group gifting
  IT: Cinque libri / Per classi scolastiche o regali multipli

Includes list item labels:
  EN: signed hardcover copy / signed hardcover copies /
      all copies shipped together / your name in the acknowledgements /
      your school's name in the acknowledgements /
      digital activity sheet (PDF) / printed educator activity guide
  IT: copia con copertina rigida firmata / copie con copertina rigida firmate /
      tutte le copie spedite insieme / il tuo nome nei ringraziamenti /
      il nome della tua scuola nei ringraziamenti /
      scheda attività digitale (PDF) / guida per educatori (stampata)

CTA button text:
  EN: Back this reward →
  IT: Sostieni questo premio →

Login notice:
  EN: When you click a button above, you'll be taken to Kickstarter
      where you'll be asked to log in or create a free account. This
      is completely normal — Kickstarter requires an account to process
      pledges. It takes about 60 seconds and is free.
  IT: Quando clicchi uno dei pulsanti, verrai reindirizzato a Kickstarter
      dove ti verrà chiesto di accedere o creare un account gratuito.
      È assolutamente normale — Kickstarter richiede un account per
      elaborare i pledges. Ci vogliono circa 60 secondi ed è gratuito.

See all rewards link:
  EN: See all reward options on Kickstarter →
  IT: Vedi tutte le opzioni su Kickstarter →

FAQ heading:
  EN: Common questions
  IT: Domande frequenti

Q1: EN: What is Kickstarter?
    IT: Cos'è Kickstarter?
A1: EN: Kickstarter is a platform where creative projects get funded by
        people before they go to print. You pledge your support, and if
        enough backers join in, the project gets made.
    IT: Kickstarter è una piattaforma dove i progetti creativi vengono
        finanziati dalle persone prima di andare in stampa. Sostieni il
        progetto, e se abbastanza persone fanno lo stesso, il libro
        viene realizzato.

Q2: EN: When will my card be charged?
    IT: Quando verrà addebitata la mia carta?
A2: EN: Not until the campaign ends. Kickstarter collects payment only
        at the close of the campaign — and only if we hit our funding goal.
        If we fall short, your card is never charged.
    IT: Solo alla fine della campagna. Kickstarter addebita il pagamento
        solo alla chiusura della campagna — e solo se raggiungiamo
        l'obiettivo. Se non ce la facciamo, la tua carta non viene
        mai addebitata.

Q3: EN: Can I cancel my pledge?
    IT: Posso annullare il mio sostegno?
A3: EN: Yes, at any time before the campaign deadline. Just log into
        your Kickstarter account and manage your pledge. No questions asked.
    IT: Sì, in qualsiasi momento prima della scadenza della campagna.
        Accedi al tuo account Kickstarter e gestisci il tuo pledge.
        Nessuna domanda richiesta.

Q4: EN: When will the book arrive?
    IT: Quando arriverà il libro?
A4: EN: We're printing with PrintNinja and targeting delivery in [SHIP_DATE].
        All backers receive email updates throughout production.
    IT: Stiamo stampando con PrintNinja e prevediamo la consegna a
        [SHIP_DATE]. Tutti i sostenitori riceveranno aggiornamenti via
        email durante la produzione.

Q5: EN: Do you ship internationally?
    IT: Spedite a livello internazionale?
A5: EN: Yes, we ship worldwide. International shipping costs are added
        at checkout on Kickstarter. Questions? Write to hello@linesinart.com
    IT: Sì, spediamo in tutto il mondo. I costi di spedizione internazionale
        vengono aggiunti al momento del checkout su Kickstarter.
        Domande? Scrivi a hello@linesinart.com

Q6: EN: What age is this book for?
    IT: Per quale fascia d'età è pensato il libro?
A6: EN: Lines in Art is designed for ages 3–8, but the open-ended questions
        work beautifully with older children too. No art background needed
        — just curiosity.
    IT: Lines in Art è pensato per bambini dai 3 agli 8 anni, ma le domande
        aperte funzionano benissimo anche con i bambini più grandi.
        Non serve nessuna conoscenza artistica — basta la curiosità.

Final CTA heading:
  EN: Help bring Lines in Art into the world
  IT: Aiutaci a portare Lines in Art nel mondo

Final CTA body:
  EN: Every pledge brings us one step closer to print — and every child
      who opens this book gets a new way to see the world.
  IT: Ogni sostegno ci avvicina alla stampa — e ogni bambino che aprirà
      questo libro avrà un nuovo modo di guardare il mondo.

Final CTA button:
  EN: Choose your reward →
  IT: Scegli il tuo premio →

Final CTA subtext:
  EN: Campaign live now · Limited early pricing
  IT: Campagna attiva ora · Prezzi early bird limitati
```

---

## 5. Page structure

One HTML file. Vanilla HTML + CSS + minimal JS. No framework.
All CSS as `<style>` in `<head>`. All JS inline at bottom of `<body>`.

---

### Section 1 — Hero

**Background:** warm cream `#FDF6E3`
**Layout desktop:** 2-column (text left, book cover image right)
**Layout mobile:** stacked (image on top, smaller; text below)

**Content:**
- Language toggle: top-right corner, `EN | IT`, always visible
- Eyebrow (small caps, 11px, blue, letter-spacing): `NOW LIVE ON KICKSTARTER`
- H1 (Fraunces, 56px desktop / 38px mobile, blue #4356A6): `Lines in Art`
- Subhead (Fraunces italic, 22px, blue): subtitle
- Body (Instrument Sans, 17px, muted #5A5F7A): 1-sentence description
- Primary CTA button (blue bg, white text, 17px): `See the rewards ↓`
  → `#rewards`
- Secondary link (no background, blue underlined, 15px):
  `View the full Kickstarter campaign →`
  → `https://www.kickstarter.com/projects/littleoaknut/lines-in-art`
  target="_blank"
- Book cover: `<img src="assets/cover.jpg">` — this is the visual hero
- Below image in muted small text: `Ages 3–8 · Hardcover · Ships [SHIP_DATE]`

No floating elements. No stats counters. No animations in the hero.

---

### Section 2 — Inside the book

**Background:** white `#FDFAF4`
**Layout desktop:** alternating image (60% wide) + text rows
**Layout mobile:** image full width, text below, then repeat

**Images permitted (copyright-safe for a commercial page):**
- Mondrian: public domain (died 1944). Safe.
- Van Gogh: public domain (died 1890). Safe.
- DO NOT use: Picasso, Matisse, Miró (all still in copyright)

**Content:**
- Section label (11px small caps, blue): `INSIDE THE BOOK`
- H2 (Fraunces, 38px): `Every spread is a conversation starter`
- Intro paragraph (17px, max 2 sentences)
- Spread 1 — Mondrian: image + artist tag + 2-sentence caption
- Spread 2 — Van Gogh: image + artist tag + 2-sentence caption
- Captions are EXPERIENTIAL not descriptive (see copy in section 4)

If images not yet available: clearly marked placeholder divs with
inline HTML comment `<!-- REPLACE: assets/spread-mondrian.jpg -->`

---

### Section 3 — How Kickstarter works

**Background:** `#4356A6` (brand blue) — ONLY dark section on the page
**Text:** white headings, rgba(255,255,255,0.80) body
**Position:** BEFORE the rewards — visitors need this context first

**Layout desktop:** 3 steps in a row
**Layout mobile:** 3 steps stacked

**Step anatomy:**
- Number in a ring (yellow #FFBD05 border, 2px, transparent fill, 48px diameter)
- Step title (Fraunces, 20px, white)
- Step body (Instrument Sans, 16px, 80% white)

**Below steps — centered, 14px, 60% white:**
Reassurance line about cancellation and no-charge-if-unfunded.

---

### Section 4 — Rewards

**id="rewards"** ← scroll target for all CTA buttons
**Background:** warm cream `#FDF6E3`

**Header:**
- Label (small caps, blue): `CHOOSE YOUR REWARD`
- H2 (Fraunces, 38px): `Three options. All Kickstarter-exclusive pricing.`
- Subtext (Fraunces italic, 17px, muted): direct links subtext

**3-card grid (desktop horizontal, mobile stacked):**

TIER 1 — Single book
```
Reward URL: https://www.kickstarter.com/projects/littleoaknut/lines-in-art/rewards#reward-UmV3YXJkLVVtVjNZWEprTFRFd056YzNOVGcy
Discount badge (orange pill, top-right): "10% off"
Card name: "One Book"
Tagline: "A single signed hardcover"
Price: [TIER1_PRICE] (large) with [TIER1_ORIG_PRICE] crossed out
Includes (green checkmarks):
  ✓ 1 signed hardcover copy
  ✓ Your name in the acknowledgements
  ✓ Digital activity sheet (PDF)
CTA button: outline style — blue border + blue text
```

TIER 2 — 3-book bundle (HERO CARD)
```
Reward URL: https://www.kickstarter.com/projects/littleoaknut/lines-in-art/rewards#reward-UmV3YXJkLVVtVjNZWEprTFRFeE1EY3lOVEUy
"Most popular" badge: centered pill above card top — blue bg, yellow text
Discount badge (orange pill, top-right): "35% off"
Scarcity label (orange text, 13px, below price): "Only a few left at this price"
Card name: "Three Books"
Tagline: "Best value — perfect for gifting"
Price: [TIER2_PRICE] (large) with [TIER2_ORIG_PRICE] crossed out
Includes (green checkmarks):
  ✓ 3 signed hardcover copies
  ✓ All 3 shipped together
  ✓ Your name in the acknowledgements
  ✓ Digital activity sheet (PDF)
CTA button: filled blue bg, white text, slightly larger than other cards
Hero treatment: 2px solid #4356A6 border, extra vertical padding
```

TIER 3 — 5-book pack
```
Reward URL: https://www.kickstarter.com/projects/littleoaknut/lines-in-art/rewards#reward-UmV3YXJkLVVtVjNZWEprTFRFeE1EZ3hOREUw
Discount badge (orange pill, top-right): "34% off"
Card name: "Five Books"
Tagline: "For classrooms or group gifting"
Price: [TIER3_PRICE] (large) with [TIER3_ORIG_PRICE] crossed out
Includes (green checkmarks):
  ✓ 5 signed hardcover copies
  ✓ Printed educator activity guide
  ✓ Your school's name in the acknowledgements
  ✓ Digital activity sheet (PDF)
CTA button: outline style — green (#568203) border + green text
```

**Login notice block — below all 3 cards:**
- Background: light blue tint `#EEF1F9`
- Left border: 3px solid `#4356A6`
- Padding: 20px 24px
- Icon: inline SVG info circle (blue)
- Text: 15px Instrument Sans, --lia-text color
- Content: login notice copy (see bilingual strings above)
- This is NOT fine print. It must be clearly visible.

**"See all rewards" link — below login notice:**
- Plain text link, underlined, blue
- Opens KS campaign URL in new tab

---

### Section 5 — FAQ

**Background:** white `#FDFAF4`
**Layout desktop:** 2-column grid
**Layout mobile:** single column
**NO accordion** — all questions and answers permanently visible

Q in Fraunces 18px blue bold.
A in Instrument Sans 16px muted, line-height 1.7.
28px gap between Q and A within a pair.
48px gap between pairs.

6 Q&A pairs (copy in bilingual strings section above).

---

### Section 6 — Final CTA

**Background:** warm cream `#FDF6E3`
**Layout:** centered, 100px padding top/bottom, max-width 600px

- H2 (Fraunces italic, 40px, blue): final CTA heading
- Body (Instrument Sans, 17px, muted, max-width 480px): 2 sentences
- CTA button (blue, large, 18px): `Choose your reward →` → `#rewards`
- Small muted text (14px): campaign status line

---

### Footer

**Background:** `#4356A6` (blue)
**Text:** white / 70% white

```
Little Oaknut  [small, Fraunces]

[linesinart.com]  ·  [Kickstarter campaign]  ·  [@linesinartbook]  ·  [hello@linesinart.com]

© 2025 Little Oaknut · Made with care in San Francisco
```

---

## 6. Interaction & motion

This page must work perfectly for a grandparent on an iPhone SE
on a slow mobile connection. Keep motion minimal.

- **Scroll reveal:** `IntersectionObserver` fade-up on section headings
  and cards. `opacity 0→1` + `translateY 20px→0`, 500ms, stagger 80ms.
- **Button hover:** `translateY(-2px)`, 180ms ease.
- **Card hover:** `translateY(-3px)`, 200ms ease.
- **Language toggle:** instant, no animation.
- **`prefers-reduced-motion`:** disables ALL transitions and animations.
- No parallax. No scroll-jacking. No spinners.

---

## 7. Technical spec

```
Output:       index.html (single file) + assets/ folder
Fonts:        Google Fonts CDN, font-display: swap
JS:           Vanilla, ~80 lines, inline at </body>
              Responsibilities: language switch + scroll reveal only
CSS:          Custom properties in :root, inline <style> in <head>
              No Tailwind, no external CSS files
Images:       Lazy loading on all below-fold images
              Explicit width + height attributes (prevent layout shift)
Responsive:   Mobile-first
              640px: tablet adjustments
              1024px: full desktop layout
Meta:         <title>, <meta name="description">,
              og:title, og:description, og:image (→ cover.jpg),
              <meta name="viewport" content="width=device-width,initial-scale=1">
              <html lang=""> updated by JS on language switch
Accessibility: Semantic HTML5 (header, main, section, footer, nav)
               h1–h6 hierarchy respected
               All images: descriptive alt text in both languages
               (use data-alt-en / data-alt-it, update via JS)
               All links: descriptive text (no "click here")
               Focus rings visible on all interactive elements
               WCAG 2.1 AA color contrast minimum
```

---

## 8. Placeholders (fill before publishing)

All appear as visible `[PLACEHOLDER_NAME]` text in the rendered
page AND as `<!-- [PLACEHOLDER_NAME] -->` HTML comments.

| Placeholder | What to put here |
|---|---|
| `[SHIP_DATE]` | e.g. "December 2025" / "Dicembre 2025" |
| `[TIER1_PRICE]` | Single book discounted price, e.g. "$27" |
| `[TIER1_ORIG_PRICE]` | Single book original price (will be crossed out) |
| `[TIER2_PRICE]` | 3-book bundle discounted price |
| `[TIER2_ORIG_PRICE]` | 3-book bundle original price (crossed out) |
| `[TIER3_PRICE]` | 5-book pack discounted price |
| `[TIER3_ORIG_PRICE]` | 5-book pack original price (crossed out) |

**Already resolved — hardcode these:**
- All 3 reward deep-link URLs (see section 5 above)
- Campaign URL: `https://www.kickstarter.com/projects/littleoaknut/lines-in-art`
- Instagram: `https://www.instagram.com/linesinartbook`
- Email: `hello@linesinart.com`
- Website: `https://linesinart.com`

---

## 9. What NOT to build

- No navigation bar or hamburger menu
- No video embed
- No Instagram feed
- No email capture form
- No cookie consent banner
- No chat widget
- No animated illustration
- No dark mode
- No countdown timer JS
- No accordion FAQ — everything visible always

---

## 10. File structure

```
/
├── index.html
└── assets/
    ├── cover.jpg             ← book cover (provide hi-res)
    ├── spread-mondrian.jpg   ← interior spread — Mondrian only
    └── spread-vangogh.jpg    ← interior spread — Van Gogh only
```

**Hosting:** Cloudflare Pages. Route via existing Worker at
`linesinart.com/back` or deploy to `back.linesinart.com`.

---

## 11. Remaining open questions

- [ ] **Prices:** exact discounted price + original price for all
      three tiers (needed for the crossed-out comparison display).
- [ ] **Ship date:** month and year for delivery estimate.

Everything else is resolved in this plan.

---

## 12. Prompt to start Claude Code

Use this exactly as your first message:

```
Read plan.md carefully and in full before writing any code.

When done reading, confirm back to me:
1. The three reward URLs you will use (paste them)
2. The four brand hex colors and how each is used
3. The two font families
4. How language switching works (one sentence)
5. Which artist spreads are permitted and why

Only after I confirm your understanding, begin building index.html.
Use visible [PLACEHOLDER_NAME] text for any unresolved values.
Do not deviate from the plan without flagging it first.
```
