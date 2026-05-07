# WNY Shed Movers — Project Brief for Claude Code

## What This Project Is
A full web presence and admin management system for **WNY Shed Movers**, a shed moving and transport business serving all of Western New York.

---

## Current File Structure
```
wny-shed-movers/
├── index.html          ← Public landing page (4 pages in one file)
├── admin/
│   └── index.html      ← Admin panel
└── PROJECT.md          ← This file
```

---

## Tech Stack
- **Frontend:** Vanilla HTML, CSS, JavaScript (no framework)
- **Database:** Supabase (PostgreSQL)
- **Hosting:** Netlify (planned) connected to GitHub for auto-deploy
- **Maps:** Leaflet.js + OpenStreetMap + OSRM (free, no API key)
- **Payments:** Stripe (planned, not yet built)
- **CRM:** GoHighLevel (planned, not yet built)

---

## Supabase Configuration
- **Project URL:** `https://mkcuvwcfjdzqlaclyjwr.supabase.co`
- **Anon Key:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1rY3V2d2NmamR6cWxhY2x5andyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzgxMTU3NTQsImV4cCI6MjA5MzY5MTc1NH0.HEv2zNNqGE75dfqasbS7oLkLH0euNb986tXw_2avXIw`
- **Database Table:** `jobs`

### Jobs Table Schema
```sql
id              uuid (primary key, auto-generated)
created_at      timestamptz (auto)
customer_name   text
customer_email  text
customer_phone  text
pickup_address  text
delivery_address text
shed_size       text  (e.g. "10x12")
shed_type       text  (e.g. "Wood", "Vinyl", "Metal")
special_instructions text
quote_amount    numeric
scheduled_date  date
assigned_driver text
status          text  (default "New")
notes           jsonb (array of {text, timestamp, author})
```

### Job Status Flow
`New` → `Contacted` → `Quoted` → `Accepted` → `Scheduled` → `Enroute` → `Completed`

---

## Public Website (index.html)
A single HTML file containing 4 pages switched by JavaScript:

### Pages
1. **Home** (`/`) — Hero with 3-step quote form, trust bar, stats, services grid, why choose us, service area, photo gallery, testimonials, CTA band, footer
2. **Buffalo Shed Movers** — City landing page targeting keyword "shed movers Buffalo NY" with nearby towns (Amherst, Cheektowaga, Tonawanda, West Seneca, Hamburg, Orchard Park, Lancaster)
3. **Rochester Shed Movers** — City landing page targeting keyword "shed movers Rochester NY" with nearby towns (Greece, Irondequoit, Webster, Henrietta, Chili, Brockport)
4. **Contact** — Contact info + same 3-step quote form as homepage

### Quote Form (both homepage and contact page)
3-step form that saves directly to Supabase `jobs` table:
- Step 1: Name, phone, email
- Step 2: Shed size, shed type, special instructions
- Step 3: Pickup address, delivery address, preferred date
- On submit: creates job with status "New", shows success confirmation

### Design
- Fonts: Oswald (headings) + Source Sans 3 (body) from Google Fonts
- Colors: Navy (#0b1f3a), Blue (#1a4f8a), Gold (#e8a020), White
- Style: Professional, blue-collar, trustworthy. Bold headings. Not flashy.
- Mobile responsive with hamburger nav

### SEO
- Unique page titles and meta descriptions per page (switched by JS)
- LocalBusiness schema markup
- Keyword-optimized content for Buffalo and Rochester local search
- Service area cities listed throughout

### TODO for public site
- [ ] Split into separate HTML files for proper SEO (each page gets its own URL)
- [ ] Add real job photos to gallery (replace emoji placeholders)
- [ ] Update phone number from (716) 555-0100 to real number
- [ ] Update email from info@wnyshedmovers.com to real email
- [ ] Add Google Business Profile link
- [ ] Add more city landing pages (Niagara Falls, Lockport, Batavia)

---

## Admin Panel (admin/index.html)
A full job management dashboard. Requires no login currently (login to be added later).

### Pages/Views
1. **Dashboard** — Metric cards (total jobs, new, scheduled this week, completed, revenue), bar chart by status, recent jobs feed
2. **Job Board (Kanban)** — Columns for each status, color-coded cards, click to open job detail
3. **Jobs List** — Sortable table of all jobs
4. **Customers** — Unique customers derived from jobs with total value

### Job Detail Page
When a job card is clicked, opens a full detail page with:
- Status selector (pill buttons for each status)
- All job fields editable inline
- Route map (Leaflet + OpenStreetMap + OSRM) showing pickup → delivery with distance and drive time
- "Open in Google Maps" button for drivers
- Notes section (add timestamped notes)
- Save and Delete buttons

### Route Map
- Uses Leaflet.js loaded from CDN
- OpenStreetMap tiles (free)
- Nominatim API for geocoding addresses
- OSRM API for routing and distance/time calculation
- Shows green pickup marker, red delivery marker, blue route line
- Summary bar: distance in miles, estimated drive time

### TODO for admin panel
- [ ] Add login/authentication (Supabase Auth)
- [ ] Stripe integration — "Send Payment Link" button on job detail
- [ ] GoHighLevel webhook — when quote submitted, create lead in GHL automatically
- [ ] Daily route planner page at /admin/routes — multi-stop optimized routes for drivers
- [ ] Driver mobile view — simplified view showing only their assigned jobs for the day
- [ ] Email/SMS notifications when new quote comes in
- [ ] Export jobs to CSV

---

## Planned Integrations

### Stripe (not yet built)
- Add "Send Payment Link" button to job detail page in admin
- When clicked, creates a Stripe payment link for the quote amount
- Customer receives link via SMS/email and pays online
- Webhook updates job status to "Accepted" when payment received
- Need: Stripe account, publishable key, secret key, webhook endpoint

### GoHighLevel (not yet built)
- When a quote form is submitted on the public site, fire a webhook to GHL
- Creates a new contact/lead in GHL pipeline automatically
- GHL handles follow-up SMS, email sequences, review requests
- Need: GHL account, webhook URL from GHL pipeline settings

### Multi-stop Route Planner (not yet built)
- New page at /admin/routes
- Drag and drop multiple jobs onto a driver's daily route
- OSRM trip service calculates most efficient order
- Single map showing full optimized route
- Estimated total mileage and drive time for the day

---

## Business Info
- **Business Name:** WNY Shed Movers
- **Service Area:** All of Western New York
- **Primary Markets:** Buffalo, Rochester, Niagara Falls, Lockport, Batavia, Albion, Medina, Jamestown
- **Phone:** (716) 555-0100 ← REPLACE WITH REAL NUMBER
- **Email:** info@wnyshedmovers.com ← REPLACE WITH REAL EMAIL
- **Admin URL:** /admin

---

## Hosting Plan
- **Host:** Netlify (free tier)
- **Repo:** GitHub → `wny-shed-movers` repository
- **Deploy:** Auto-deploy on GitHub push
- **Domain:** Custom domain to be connected (TBD)

---

## How to Continue in Claude Code

When starting a new Claude Code session, say:
*"Read PROJECT.md and continue building the WNY Shed Movers project"*

Claude Code will read this file and have full context on everything built so far, what's planned, and what needs to be done next.

### Recommended next steps in Claude Code:
1. Set up a local dev server (`npx serve .`) so you can preview changes in browser
2. Fix phone number and email throughout both files
3. Split index.html into separate pages for SEO
4. Add Supabase Auth to admin panel
5. Build Stripe payment link integration
6. Build GoHighLevel webhook on form submit
7. Build the daily route planner page
