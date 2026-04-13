# Knightify - Lovable Alignment Prompt

## Context

This app already exists as a single-page site with a beautiful medieval illuminated manuscript aesthetic. **Do NOT change the visual design language** - keep the blackletter fonts, parchment backgrounds, gold ornamental borders, fleur-de-lis (⚜) decorations, and Old English-flavoured copy.

The task is to **expand this into a full multi-page app** that matches the architecture of its sibling white-label sites (Soldierify, Pirateify, Mythify). Same backend, same API endpoints, same auth flow, same feature set - just with the existing Knightify medieval aesthetic applied throughout.

**Live reference site:** https://soldierify.net (use this to see the full UX patterns, page structure, and flow that Knightify needs to match)

---

## Tech Stack (align with siblings)

- React 19 + TypeScript
- Vite
- Tailwind CSS
- Supabase (Auth + Database + Storage)
- Stripe (Payments)
- React Router v6
- Zustand (state management)
- Lucide React (icons)

---

## Existing Aesthetic to PRESERVE

### What's already working well - DO NOT CHANGE:
- **Blackletter/medieval fonts** for headings and titles
- **Parchment/cream background** (`#f5f0e8` or similar warm cream)
- **Gold ornamental borders** with corner flourishes on cards
- **Illuminated manuscript-style header** illustration
- **Fleur-de-lis (⚜) ornaments** as decorative dividers
- **Old English copy style** ("Thy Legend Awaiteth", "Choose Thy Knight", "Hearken, good gentles")
- **Warm earth-tone palette** (cream, gold, deep brown, parchment)
- **Medieval card styling** with gold borders and hover effects
- **Footer** with knight imagery and medieval disclaimer text

### Color Palette (extract from existing)
```
Backgrounds:
- knight-bg-primary: #f5f0e8 (warm parchment)
- knight-bg-secondary: #ebe4d4 (darker parchment)
- knight-bg-card: #faf7f0 (light parchment card)
- knight-bg-dark: #2c1810 (dark brown for header/footer)
- knight-bg-darker: #1a0f08 (deepest brown for modals/overlays)

Gold Accents:
- knight-gold-400: #c9a84c (primary gold)
- knight-gold-500: #b8922a (standard gold)
- knight-gold-600: #9a7a22 (hover gold)
- knight-gold-border: #d4b45e (card border gold)

Text:
- text-primary: #2c1810 (dark brown on light backgrounds)
- text-primary-inverted: #f5f0e8 (cream on dark backgrounds)
- text-secondary: #5c4a3a (medium brown)
- text-muted: #8a7a6a (muted brown)
- text-gold: #c9a84c

Borders:
- border-default: #d4c4a8 (parchment border)
- border-active: #c9a84c (gold)
- border-card: #d4b45e (gold card border)

Status/Action Colors:
- accent-red: #8b1a1a (deep heraldic red)
- accent-blue: #1a3a6b (heraldic blue)
- success: #2d5a27 (forest green)
- error: #8b1a1a
```

### Typography
- Keep the existing blackletter/medieval display font for headings
- Body text: serif font (Georgia, Garamond, or similar) for readability
- Custom text utility classes:
  - `.knight-heading` - blackletter, uppercase or title case
  - `.knight-subheading` - smaller medieval styling
  - `.knight-caption` - small caps, letter-spacing
  - `.knight-body` - readable serif for body paragraphs

---

## Pages & Routes TO ADD

The current site is a single page. Expand to these routes matching the Soldierify architecture:

### 1. Home Page (`/`) - REFACTOR EXISTING
Keep the existing home page content but restructure it:

**Hero Section** (keep existing illuminated manuscript header + medieval copy):
- Keep: "Knightify" title, "Thy Legend Awaiteth" tagline, medieval description
- CTA Button (logged out): "Sign In to Begin Thy Quest" with LogIn icon
- CTA Button (logged in): "Begin Thy Quest" with ArrowRight icon

**Face Swap Section** (keep existing before/after examples):
- Keep: "Not just art about you. Art with you in it." section
- Keep: The 4 before/after comparison cards

**Features Section** (add below face swap, match siblings):
- Section label: "⚜ Features ⚜"
- Section title: "Transformations of Renown"
- Three feature cards:
  1. **Icon:** Shield | **Title:** "Historical Authenticity" | **Description:** "Nine warriors of diverse station & provenance, meticulously researched from the annals of history."
  2. **Icon:** Clock | **Title:** "Swift as an Arrow" | **Description:** "Thy transformed portrait shall be rendered in under thirty seconds by our engine of transfiguration."
  3. **Icon:** Palette | **Title:** "Fit for a Gallery" | **Description:** "High-resolution outputs worthy of printing upon canvas, parchment, or fine merchandise."

**Warriors Grid** (keep existing "The Nine Warriors" section):
- Keep the 3x3 grid of warrior cards with gold borders
- Update image URLs to Supabase storage (see Image URLs section below)

**SEO Structured Data:**
```json
{
  "@context": "https://schema.org",
  "@type": "WebApplication",
  "name": "Knightify",
  "url": "https://knightify.net",
  "description": "Transform thy portrait into a legendary warrior using AI. Knights, samurai, Vikings, hussars, and more elite fighters from across history.",
  "applicationCategory": "MultimediaApplication",
  "operatingSystem": "Web",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "description": "Free transformations available with credit system"
  },
  "featureList": [
    "AI-powered face transformation",
    "9 historical warrior templates",
    "Warriors from Europe, Asia, Middle East, Americas",
    "High-quality portrait generation",
    "Print ordering service",
    "Community gallery sharing"
  ]
}
```

**SEO Meta:**
- Title: "Knightify — Transform Thy Portrait into Legend"
- Description: "Use AI to see thyself as a legendary warrior from history. Knights, samurai, Vikings, hussars, and more. Create thy transformation in seconds."
- Keywords: "AI face swap, knight transformation, warrior portrait, medieval portrait generator, AI photo transformation, historical warriors"

---

### 2. Transform Page (`/transform`) - NEW
**This is the core page - most complex.**

**Layout:** 2-column on desktop (stacks on mobile)
- Left column: Image uploader + Warrior selector
- Right column: Selected warrior preview (sticky on desktop) + Transform button + Results

**Components:**
1. **CreditsDisplay** - Shows remaining credits, subscription tier, "Purchase Credits" button (styled as parchment badge)
2. **ImageUploader** - Drag-drop zone styled as a parchment scroll + "Capture Thy Visage" camera button (mobile). Accepts JPEG, PNG, WebP. Max 10MB. Shows progress bar styled as a medieval loading bar.
3. **WarriorSelector** (replaces SoldierSelector) - Browse warriors with:
   - Search bar (styled as medieval input with gold border)
   - Category filter dropdown (All, European Warriors, Asian Warriors, Middle Eastern Warriors, Americas Warriors)
   - Grid view of warrior cards matching the home page style
   - Each card shows: image, name, era tag, description preview
4. **TransformPreview** - Shows selected warrior + uploaded photo side by side in gold-bordered frames
5. **Transform Button** - "⚜ Forge Thy Portrait ⚜" / disabled states
6. **TransformationProgress** - Real-time progress bar with WebSocket connection, percentage, medieval status text ("The alchemists work their craft...", "Thy portrait takes form...")
7. **Result Actions** (after transform completes):
   - Download button ("Claim Thy Portrait")
   - "Order a Print" button (opens PrintOrderModal)
   - "Share with the Realm" button
   - "Rate This Work" button (opens FeedbackForm)

**SEO Meta:**
- Title: "Forge Thy Warrior Portrait"
- Description: "Upload thy photo and transform into a Gothic Knight, Samurai Commander, Viking Chieftain, and more legendary warriors."

---

### 3. Gallery Page (`/gallery`) - NEW
**Layout:** Collapsible sections organized by category, styled with gold ornamental borders

**Categories (collapsible sections with ⚜ dividers):**
- ⚜ European Knights & Warriors (Gothic Knight, Varangian Guard, Winged Hussar, Viking Chieftain, Scottish Highlander)
- ⚜ Asian Warriors (Samurai Commander, Mongol Kheshig)
- ⚜ Middle Eastern Warriors (Mamluk Cavalryman)
- ⚜ Americas Warriors (Aztec Jaguar Warrior)

**Each warrior shows:**
- Image with zoom on hover (magnifying glass overlay)
- Warrior name in blackletter
- Era/period tag
- Gold gradient border around image
- Click to open zoom modal
- Pagination: 12 per page per section

**Layout Switcher:** Keep simple - Categories Layout (default) and Classic Grid

**SEO Meta:**
- Title: "Gallery of Warriors"
- Description: "Explore our collection of legendary warriors from across history and the globe."

---

### 4. Community Page (`/community`) - NEW
**Layout:** Split-screen - Gallery (2/3) + Forum sidebar (1/3)

**Gallery Section (left, lg:col-span-2):**
- Grid of community-shared transformations
- Sort: Latest / Most Popular
- Each card shows: transformed image, username, warrior used, like count
- Like/unlike button (heart icon)
- Report button (flag icon)
- Status badges: Pending / Approved / Rejected / Flagged (styled as medieval seals)

**Forum Sidebar (right, lg:col-span-1, sticky on desktop):**
- "⚜ The Great Hall ⚜" header (in blackletter)
- Latest 10 forum posts (title, author, reply count, time ago)
- "Post a Proclamation" button
- "View All Posts" link → goes to /forum
- On mobile: stacks below gallery, not sticky

---

### 5. My Gallery Page (`/my-gallery`) - NEW
**Layout:** Grid of user's own transformations

**Features:**
- Grid: 1-col mobile, 2-col tablet, 3-4 col desktop
- Status filters: All / Completed / Processing
- Each card shows:
  - Transformed image in gold-bordered frame
  - Warrior name + era
  - Status badge (styled as wax seal)
  - Date created
  - Action buttons: Download, Share, Order Print, Share to Community, Delete
- "Load More" pagination

**Nav link text:** "My Gallery"

---

### 6. Profile Page (`/profile`) - NEW
**Sections (styled as parchment cards):**
- Account info (email, display name)
- Subscription status (Free / Monthly / Annual) - shown as medieval rank (Peasant / Squire / Knight)
- Credits remaining
- "Upgrade Thy Rank" button → opens PaymentModal
- "Manage Subscription" → Stripe billing portal
- Order History button → modal showing past print orders
- Sign Out button

---

### 7. Admin Dashboard (`/admin`) - NEW
**Only visible to admin users. Sections:**
- System Health (API status, DB status, uptime)
- Transformation Stats (total, today, this week, this month, estimated costs)
- AI Provider Status (Replicate availability, health, cost per transform)
- Template Manager (CRUD for warrior templates)
- Community Moderation (approve/reject/flag shared transformations)
- Forum Moderation (pin/remove posts)
- Feedback Reviews (user ratings and comments)

---

### 8-13. Additional Pages - NEW
Same structure as reference site:
- `/forum` - Forum listing ("The Great Hall") (sort: Latest/Popular/Trending)
- `/forum/:postId` - Individual forum post with replies
- `/view/:id` - Public shareable transformation view with QR code
- `/privacy` - Privacy Policy (use "Knightify" throughout)
- `/terms` - Terms of Service
- `/refunds` - Refund Policy
- `/subscription-success` - Payment success page ("Thy Rank Hath Been Elevated!")
- `/print/success` - Print order success
- `/print/cancel` - Print order cancelled
- `/auth/callback` - OAuth redirect handler

---

## Navigation & Layout

### Header (all pages)
```
[Shield icon] KNIGHTIFY    [Camera icon] Transform    Home | Gallery | Transform | Community | My Gallery | [User icon / Sign In]
```
- Logo: Shield icon (lucide) + "KNIGHTIFY" text in blackletter
- Quick Transform button with Camera icon (hidden on mobile)
- Desktop nav links in medieval styling (hidden on mobile, shown md+)
- Mobile: hamburger menu (MobileNav component)
- Style: dark brown background matching existing header/footer

### Footer (all pages)
Keep the existing medieval footer style with knight imagery:
```
⚜ Knightify ⚜
Transform thy portrait into legend. All knight imagery is AI-generated for illustrative purposes.
© 2025 Knightify. All rights reserved.
Privacy Policy | Terms of Service | Refund Policy
```

---

## Authentication - NEW

- Supabase Auth with email/password
- AuthModal component styled as a parchment scroll (sign up / sign in toggle)
- Email confirmation required
- Auth state in Zustand store: `{ user, isLoading, isAdmin }`
- `onAuthStateChange` listener
- Protected routes redirect to auth modal
- Admin detection via `/api/auth/me` endpoint

---

## Payment System - NEW

### PaymentModal - 3 tiers (styled as medieval ranks):
1. **5 Credit Pack** - $2.99 (one-time)
   - Rank: "Page"
   - "Perfect for a first quest"
   - 5 transformation credits
2. **Monthly Unlimited** - $9.99/month
   - Rank: "Squire"
   - "Unlimited transformations"
   - All warrior templates
   - Priority processing
3. **Annual Unlimited** - $99.00/year (save 17%)
   - Rank: "Knight"
   - Everything in monthly
   - Best value
   - "The most noble path"

---

## Print Order System - NEW

### PrintOrderModal - 5 product categories:
1. **Wall Art** (Package icon) - "Tapestries, canvas, framed prints"
2. **Apparel** (Shirt icon) - "Tunics & hooded cloaks" (T-shirts, hoodies)
3. **Drinkware** (Coffee icon) - "Goblets & tankards" (Mugs, travel cups)
4. **Accessories** (Smartphone icon) - "Phone cases, bags"
5. **Greeting Cards** (Gift icon) - "Scrolls of greeting" (Birthday, holiday cards)

---

## The 9 Warrior Templates

### Image URLs (Supabase Storage)
All images are served from: `https://oxwqlophcufdietpfuvc.supabase.co/storage/v1/object/public/knight-bases/`

**Update all warrior image references to use these URLs:**

### European Knights & Warriors

**1. Gothic Knight**
- Era: 15th Century
- Region: Central Europe
- Image: `gothic_knight_tournament.png`
- Description: "The Gothic knight represents the height of late medieval armour design. Clad in full plate armour with distinctive fluted surfaces, these knights were the elite warriors of European feudal society, trained from youth in the arts of war and chivalry."

**2. Varangian Guard**
- Era: 10th-14th Century
- Region: Byzantine Empire / Constantinople
- Image: `varangian_guard_constantinople.png`
- Description: "The Varangian Guard was an elite unit of the Byzantine Army. Composed primarily of Norse and Anglo-Saxon warriors, they served as bodyguards to the Byzantine Emperor and were famed for their loyalty, ferocity, and iconic Dane axes."

**3. Winged Hussar**
- Era: 16th-18th Century
- Region: Poland-Lithuania
- Image: `hussar_cavalry_charge.png`
- Description: "The Winged Hussars were the elite cavalry of the Polish-Lithuanian Commonwealth. Famous for their distinctive feathered wings and devastating shock charges, they were considered the most powerful cavalry in Europe."

**4. Viking Chieftain**
- Era: 8th-11th Century
- Region: Scandinavia
- Image: `viking_chieftain_coast.png`
- Description: "Viking chieftains were the warrior-leaders of Norse society during the Viking Age. They led longship expeditions across Europe, combining fearsome martial prowess with political cunning and seamanship."

**5. Scottish Highlander**
- Era: 14th-18th Century
- Region: Scottish Highlands
- Image: `scottish_highlander_glen.png`
- Description: "The Scottish Highland warriors were the clansmen renowned for their fierce independence, clan loyalty, and devastating Highland charges. Armed with claymores, targes, and dirks."

### Asian Warriors

**6. Samurai Commander**
- Era: 12th-19th Century
- Region: Japan
- Image: `samurai_commander_spring.png`
- Description: "The samurai were the military nobility of medieval and early-modern Japan. As commanders, they led armies with martial discipline, strategic brilliance, and adherence to bushido — the way of the warrior."

**7. Mongol Kheshig**
- Era: 13th-14th Century
- Region: Mongolia / Central Asia
- Image: `mongol_kheshig_steppes.png`
- Description: "The Kheshig were the imperial guard of the Mongol Empire, serving as bodyguards to the Great Khan. These elite warriors were the finest horsemen and archers in the largest contiguous land empire in history."

### Middle Eastern Warriors

**8. Mamluk Cavalryman**
- Era: 13th-16th Century
- Region: Egypt / Syria
- Image: `mamluk_cavalryman_cairo.png`
- Description: "The Mamluks were a warrior caste of slave-soldiers who rose to rule Egypt and Syria. Superb horsemen who famously defeated both the Crusaders and the Mongol invasion at Ain Jalut in 1260."

### Americas Warriors

**9. Aztec Jaguar Warrior**
- Era: 14th-16th Century
- Region: Mesoamerica / Aztec Empire
- Image: `aztec_jaguar_warrior_temple.png`
- Description: "The Jaguar Warriors were the elite military order of the Aztec Empire. Only the bravest soldiers who had captured multiple enemies could join their ranks. They fought with obsidian-edged macuahuitl clubs."

---

## API Integration

The frontend connects to a Node.js/Express backend. All authenticated requests include a Supabase JWT bearer token.

### API Client Setup
- Base URL from `VITE_API_URL` environment variable
- Auth headers: `Authorization: Bearer <supabase_jwt_token>`
- Retry logic: 3 retries with exponential backoff (500ms base)

### Key Endpoints
```
# Auth
GET  /api/auth/me           - Get user profile + admin status
GET  /api/auth/profile      - User profile data
PUT  /api/auth/profile      - Update profile

# Transform
POST /api/transform/upload  - Upload photo + start transformation
GET  /api/transform/status/:id - Poll transformation progress
GET  /api/transform/history - User's transformation history
GET  /api/transform/credits - User credits info
PATCH /api/transform/:id/visibility - Toggle public/private

# Gallery
GET  /api/gallery/transformations - User's gallery
GET  /api/gallery/download/:id    - Get download URL
POST /api/gallery/share/:id       - Create share link
DELETE /api/gallery/:id           - Delete transformation

# Payment
POST /api/payment/create-credit-checkout      - Stripe checkout for credits
POST /api/payment/create-subscription-checkout - Stripe checkout for subscription
GET  /api/payment/orders                       - Order history
GET  /api/payment/billing-portal               - Stripe billing portal link

# Print
GET  /api/print/catalog     - Product catalog
POST /api/print/checkout    - Create print order checkout
GET  /api/print/orders/mine - Print order history

# Forum
GET  /api/forum/posts           - List posts
POST /api/forum/posts           - Create post
GET  /api/forum/posts/:id       - Get post with replies
POST /api/forum/posts/:id/replies - Add reply

# Analytics (admin)
GET  /api/analytics/health              - System health
GET  /api/analytics/transformation-stats - Stats + costs
```

### WebSocket
- Real-time transformation progress tracking
- Connects during transformation, shows percentage + status updates

---

## Responsive Design

- **Mobile-first** approach
- Breakpoints: sm (640px), md (768px), lg (1024px)
- Mobile: single column, bottom nav, camera capture
- Tablet: 2-column layouts
- Desktop: 3-4 column grids, sticky sidebars, hover effects
- All tap targets 44px+ on mobile

---

## State Management

- **Zustand** store for auth: `{ user, isLoading, isAdmin, setUser, setLoading, setIsAdmin }`
- **React Context** for toast notifications (styled as medieval parchment toasts)
- **Custom hooks:**
  - `useAuthStore()` - Auth state
  - `useToast()` - Toast notifications
  - `useTemplates(themeSlug)` - Fetch templates
  - `useCurrency()` - Format prices with local currency

---

## Environment Variables (Frontend)

```env
VITE_SUPABASE_URL=<supabase_project_url>
VITE_SUPABASE_ANON_KEY=<supabase_anon_key>
VITE_API_URL=<backend_api_url>
VITE_STRIPE_PUBLISHABLE_KEY=<stripe_pk_key>
VITE_SENTRY_DSN=<sentry_dsn>
VITE_APP_URL=https://knightify.net
```

---

## PWA / SEO Features

- Service worker registration (production only)
- PWA manifest
- Apple touch icons
- Theme color meta tag (use #2c1810 dark brown)
- Preconnect to Google Fonts
- Content Security Policy meta tag
- Per-page SEO component with title/description/keywords/url
- JSON-LD structured data on home page

---

## Key UX Patterns

1. **Loading states:** Spinner icons, skeleton loaders styled as parchment placeholders, medieval-themed progress bars
2. **Transitions:** Smooth hover effects (duration-200/300), gold border glow on hover, scale transforms on buttons
3. **Modals:** Fixed inset-0 overlay with semi-transparent dark brown, z-50, parchment-styled modal body, click-outside to close
4. **Image interactions:** Zoom on hover with magnifying glass, gallery lightbox with gold frame, lazy loading
5. **Button states:** Gold border glow on hover, disabled opacity-50 + cursor-not-allowed, active scale-95
6. **Form validation:** Red borders on error, green on success, gold on focus, inline error messages in medieval serif font
7. **Toast notifications:** Styled as small parchment scrolls - success (green seal), error (red seal), info (blue seal) - auto-dismiss

---

## Important Implementation Notes

1. **Theme slug:** Set `THEME_SLUG = 'knightify'` in `src/config/theme.ts`
2. **Keep all medieval copy** - don't modernise the language. New pages should use the same Old English-flavoured tone.
3. **Keep the blackletter headings** - all new page headings should use the same medieval display font
4. **Keep the gold ornamental borders** - cards on new pages should match the existing warrior card style
5. **Keep the parchment backgrounds** - new pages should use the same warm cream/parchment palette
6. **The images for templates are served from Supabase Storage** at `https://oxwqlophcufdietpfuvc.supabase.co/storage/v1/object/public/knight-bases/`
7. **Do NOT hardcode any API keys** - all secrets go in environment variables
8. **Keep the same component structure** as the reference site (soldierify.net)
9. **Icon swap:** Use Shield icon (from lucide) for the logo, not Sword

---

## Copy Style Guide

All new copy should follow the existing medieval tone. Examples:

| Standard Copy | Knightify Copy |
|--------------|---------------|
| "Sign In" | "Enter the Keep" |
| "Sign Up" | "Join the Order" |
| "Sign Out" | "Leave the Keep" |
| "Upload Photo" | "Submit Thy Visage" |
| "Transform" | "Forge Thy Portrait" |
| "Download" | "Claim Thy Portrait" |
| "Share" | "Share with the Realm" |
| "My Gallery" | "My Gallery" |
| "Community" | "The Realm" |
| "Forum" | "The Great Hall" |
| "Profile" | "Thy Account" |
| "Credits" | "Coin" |
| "Buy Credits" | "Purchase Coin" |
| "Subscribe" | "Pledge Thy Allegiance" |
| "Processing..." | "The alchemists work their craft..." |
| "Complete!" | "Thy portrait is forged!" |
| "Error" | "Alas! An error hath occurred" |
| "Loading" | "Summoning..." |
| "No results" | "The archives reveal naught" |
| "Delete" | "Cast into the flames" |
| "Cancel" | "Withdraw" |
| "Confirm" | "So be it" |
| "Success" | "Victory!" |

---

## What to Build (Priority Order)

1. Add React Router with all routes
2. Add persistent header/footer layout with navigation (keep existing medieval style)
3. Add Auth modal (Supabase integration, styled as parchment scroll)
4. Refactor home page into Layout + Home route
5. Build Transform page (upload + warrior selector + transform flow)
6. Build Gallery page with all 9 warriors (using Supabase storage URLs)
7. Build My Gallery page
8. Build Profile page with payment modal
9. Build Community page (gallery + forum sidebar)
10. Build Forum pages
11. Build Admin dashboard
12. Build Print order modal
13. Build Legal pages (privacy, terms, refunds)
14. Build Success/cancel pages
