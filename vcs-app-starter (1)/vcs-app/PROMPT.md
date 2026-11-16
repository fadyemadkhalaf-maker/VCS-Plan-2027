# Antigravity Prompt v2: VCS 2027 Experience

## Role
You are a senior product designer and front-end engineer. Build a premium, static single-page web app (no backend) presenting the Servier Egypt **VCS (Visit & Communication Safety) Plan, 1 Dec 2026 – 9 Sep 2027**. Audience: Engineering/HSE leadership. It must feel like a finished internal product that clearly outclasses the existing Power BI dashboard, while still feeling like the same family.

## Scope: 2027 plan ONLY
The app covers ONLY the VCS 2027 plan (1 Dec 2026 – 9 Sep 2027), which is exactly what data/vcs_data.json contains. There is no 2026 data in this project. The reference screenshot shows the OLD 2026 dashboard: use it for look and feel only, and never copy its numbers (135 / 129 / 6 etc.), names, months or statuses. Every number on screen must be computed from vcs_data.json.

## Project layout (already prepared, do not move files)
```
vcs-app/
  PROMPT.md
  index.html            <- you create
  styles.css            <- you create
  src/app.js            <- you create (logic, views)
  src/i18n.js           <- you create (ar / en / fr dictionaries)
  data/vcs_data.json    <- provided: 134 visits, 49 people, 50 locations, location rules, meta
  assets/logos/         <- provided by user (servier.png, cairo-safe.png, safe.png, hse.png, near-miss.png)
  assets/reference/Servier_VCS_Dashboard_Overview.png  <- visual reference only
```
Do not invent data. Embed the JSON into the page at build time too, so the app also works when index.html is opened directly from disk (fetch can be blocked on file://).

## Visual reference (look at assets/reference/Servier_VCS_Dashboard_Overview.png)
This is the team's existing Power BI dashboard. Keep its identity and make it far more refined:
- Header: deep navy band with Servier logo left, centered title, a "Run Flow" style primary action button, and the current date right. Keep that structure, but elevate it.
- KPI row of five colored tiles: Total (neutral gray), Today's (blue), Overdue (red), Upcoming (amber), Completed (green). Reuse this exact semantic color coding everywhere (status chips, calendar, charts).
- Palette taken from the reference (refine, do not copy blindly): navy ~#0B2545 / #12306B, text navy ~#0F2A6B, blue ~#1E90FF, red ~#E53935, amber ~#FFC233, green ~#5E9E3F, gray ~#6B6B6B, Servier accent red-orange ~#E8402A, soft backgrounds white and very light blue-gray.
- Quick Actions block with three round/pill badges: HSE (navy outline pill), Cairo Safe logo, Near Miss (red ring). Keep them as a signature element and make them nicely interactive (they can link to "#" placeholders).
- Charts in the reference: bars by department, bars by location, donut for status, area chart for monthly trend, a data table. Keep all of these types, but better: clearer labels, no truncated names, hover details.
Add department colors (5 distinct, accessible) for ENGINEERING, Quality Control, Production, Supply Chain, IT, used consistently across calendar, chips and charts.

## Important data note
In the new plan ALL 134 visits are Pending (nothing completed yet). The reference shows the old 2026 plan. So KPIs here are: Total 134, Today's (computed from today's date), Overdue (Pending with date before today), Upcoming (Pending with date after today), Completed (0 now, but fully supported). Compute everything from data and today's date, never hard-code.

## Languages
Full i18n with a language switcher: **العربية / English / Français**. Arabic switches the layout to RTL (use logical CSS properties). Translate all UI text, month/weekday names and number/date formatting via `Intl` for each language. Names of people, departments and locations stay as in the data. Remember the choice in localStorage. Use a good Arabic-capable font (for example Cairo or IBM Plex Sans Arabic) alongside the Latin font; test that mixed Arabic/Latin lines look right.

## Screens
1. **Home / Overview**: navy hero header with Servier + Cairo Safe logos, title "VCS Plan 2027", period, animated counters (134 visits, 49 employees, 50 locations, 40 weeks), the five KPI tiles, next 3 upcoming visits with a live countdown, Quick Actions block, badge "All validation checks passed" from `meta.checks`.
2. **People (star feature)**: big search box with autocomplete over all 49 names. Selecting a person opens a profile: name, department, manager, quota ring (scheduled vs required), a full-period mini calendar with their days highlighted, timeline of every visit (date, weekday, location, status), location chips. Buttons: "Print / Save as PDF" (clean personal plan), "Copy summary". Filters by department and manager; grid of all people as compact tiles.
3. **Calendar**: month view Dec 2026 → Sep 2027, work week Sunday–Thursday (Friday/Saturday muted), visits as department-colored pills, click a day to see who goes where. Filters: department, location, person. Week view with weekly distribution.
4. **Locations**: all 50 locations with visit counts, visitors and dates, heat view by count, department restrictions (e.g. Engineering not allowed at Boiler house) and review notes from `location_rules`. Click a location to filter the calendar.
5. **Insights**: visits per month, visits per week (40 weeks), department split vs required quotas (Engineering 56, QC 40, Production 20, Supply Chain 10, IT 8), top locations, manager view (each manager's team visits).
6. **Data notes** (footer/about): the review flags from `location_rules` so leadership sees open ownership questions.

## Behavior
- Global search (Ctrl/Cmd+K) to jump to a person or location. Shared filter state across views. URL hash reflects view and selection.
- Light and dark themes; both finished.
- Export CSV of filtered visits; print stylesheet for People and Calendar.
- Empty states say what to do next. No lorem ipsum.

## Design principles
- Calm, confident, pharmaceutical-grade precision. Spend boldness in ONE place: the hero/overview. Everything else quiet and disciplined. Avoid identical rounded card grids everywhere; vary structure by content.
- Two typefaces at most, with a real type scale. One orchestrated page-load moment (counters); other motion only responds to user actions; respect prefers-reduced-motion.
- Quality floor: visible keyboard focus, AA contrast, responsive to 360px, no console errors, fast first paint.
- Before finishing, take screenshots at desktop, tablet, mobile, in all three languages and both themes. Fix anything cramped, clipped or misaligned.

## Deliver
Run the app, verify each screen against this brief, then summarize what you built and anything you could not do.
