# Progress 03 – NebulaPalooza 2026

Team: Beisembayev Bexultan, Shagieva Liliana, Gulnaz Baizhaunova (ČZU Prague)

## 1. What has been implemented

The `web/` folder is a purely client-side site (HTML, CSS, JavaScript; no build step, no server). It opens by double-clicking `web/index.html`.

| Page | Content | Interactive features |
| ---- | ------- | -------------------- |
| `index.html` | Hero, countdown to opening, image carousel, concept, "What's on", 3-day overview, ticket info | Countdown, carousel |
| `programme.html` | All 60 events over 3 days. Each day is split into two columns: Letná Park (left) and Výstaviště Praha (right) | Text search, category filter, "★ Save" button per event |
| `custom_list.html` | "My Programme": events saved by the visitor | Saved events are stored in the browser (`localStorage`), shown here and can be removed |
| `performers.html` | Directory of performers by group (music, speakers, artists) and 3 spotlights | Links from spotlights to the matching programme day |
| `practical.html` | Venues, transport, accommodation, tickets, contact | Anchor navigation from the programme page |

Main navigation (header and footer) is the same on all pages and the current page is highlighted.

Data foundation: `data/festival.xml` (60 performers, 60 events, 3 days, 2 venues), `data/festival.xsd`, and a static API in `web/API/` (7 XSLT transformations, 7 JSON files, 7 JSON Schemas, OpenAPI 3.1).

Semantic HTML: `header`, `nav`, `main`, `section`, `article`, `aside`, `footer`, `time`, `address`, lists, labelled form controls, one `h1` per page.

## 2. UX and design practices and how they were applied

- **Mobile-first behaviour and responsiveness.** Layouts use CSS grid that collapses to one column; no horizontal scrolling at 320, 375, 768 and 1280 px (checked, see section 4). On narrow screens the navigation wraps under the logo, the hero heading wraps and the carousel images are shorter.
- **Programme layout by venue.** On desktop the two venues are shown side by side so a visitor can compare what is on at the same time on both stages. The order is the same on every day (Letná left, Výstaviště right). On mobile the columns stack.
- **Finding things fast.** Live search and category filter on the programme page.
- **Personal programme.** A visitor saves events with one click and sees them on a separate page; this is the first key scenario ("find an evening performance and save it").
- **Consistent navigation and visual language.** Same header, footer, colours and typography on every page (one stylesheet, CSS variables).
- **Semantic structure and metadata.** `title`, `meta description`, viewport, Open Graph and JSON-LD on the main pages.

## 3. What changed compared with the prototype and why

The previous stage is the Seminar 3 submission: a one-page interactive timetable (`index.html`, data embedded from `data.json`) with `progress-02.md`. The feedback on it was that only a timetable had been submitted and that the festival introduction, performer overview and details, venue details, practical information and a search results destination were missing. The structure of the site follows `project-specification.md` from Seminar 2.

| Prototype (Seminar 3) | Now (Seminar 4) | Why |
| --------------------- | --------------- | --- |
| One page with the timetable only | Five pages: Home, Programme, Performers, Practical info (with venues), My Programme, linked by the same navigation | Feedback on Seminar 3: festival introduction, performers, venues and practical information were missing |
| Day chips and venue chips filtered one list | Each day is its own section and the two venues are shown side by side in two columns (Letná left, Výstaviště right) | Requested at the consultation: events separated by venue. Trade-off: there is no day filter now, the visitor scrolls to the day |
| Search (name, specialty, activity) | Search by text plus a category filter (Music, Workshop, Tech, Visual, Show, Science, Talk, Art) | Category filtering by activity type is a key user action in the specification |
| "My Nebula" side drawer with a counter badge in the header | "★ Save" on each event and a separate "My Programme" page | Simpler to reach from every page. Trade-off: the counter badge was dropped |
| Events built by JavaScript from embedded data | Events written directly in the HTML | Faster to build for the multi-page site. Trade-off: the programme is no longer generated from the data file (counts match, 60 and 60) |

Other fixes in this version: the mobile layout (pages were wider than a 375 px screen), the missing category options (18 of 60 events could not be filtered) and the team names in the footer and `readme.md`.

Not yet restored from the prototype: day and venue filter chips and the header counter badge. Not yet built from the specification: separate performer detail pages, a venues page and a search results page.

## 4. Tests and checks performed

| Check | How | Result |
| ----- | --- | ------ |
| No horizontal overflow | Headless Chrome, measured page width at 320, 375, 768, 1280 px on all 5 pages | After the fix: no overflow on any page. Before the fix: overflow on 3 pages at 375 px |
| Programme data matches data foundation | Counted events in `festival.xml` and event rows in `programme.html` | 60 and 60 |
| Venue columns | Visual check of the programme page at desktop and phone width | Two columns on desktop, stacked on phone |
| Search | Browser test: "DRONE" gives 3 results, " aurora " (with spaces) gives 1, "zzzz" gives 0, an empty box shows all 60 | Works, case-insensitive |
| Category filter | Browser test of every dropdown option: Music 24, Workshop 7, Tech 7, Visual 2, Show 2, Science 5, Talk 5, Art 6. That is 58 of 60 events; 2 events with the one-off tags "Event" and "Finale" are only reachable through search | Works after adding the missing options |
| Search and filter together | Typed in the search box after choosing Music: results narrowed to the matching Music event. Choosing a category after typing clears the search box | Partly: works in one order only (see shortcomings) |
| Save, persistence, My Programme | Saved 2 events, reloaded the page (button stayed "✓ Saved"), opened My Programme (both events listed), removed one with "Remove" (storage updated) | Works |
| HTML validity, accessibility (WCAG 2.0 AA) | Not done yet | Planned for the next version |

## 5. Known shortcomings

- No site-wide search and no search results page yet.
- Performers have no individual detail pages yet.
- Venues are described inside the Practical page, not on their own page.
- Many inline styles remain; they should move to `style.css`.
- Images are very large (5–10 MB each), pages load slowly.
- Heading levels on the programme page need correction (venue and event titles are on the same level).
- Some tap targets are small on mobile.
- Ticket prices differ between `data.json` (Day Pass 25 EUR, Full Cosmic Access 60 EUR) and the site (20 EUR, 49 EUR); one source has to be chosen and the other corrected.
- The programme is written by hand in HTML, not generated from the data file.
- The page names differ from the Seminar 2 sitemap (`practical.html` instead of `info.html`; no `venues.html`, `performer-detail.html` or `search.html`).
- Choosing a category clears the search text, so the two filters are not fully combined (the Seminar 3 text says they are).
- My Programme shows only the event title and generic text ("Saved Event", "Custom Schedule"); time and venue are not shown, unlike the description in the Seminar 3 text.

## 6. Use of AI and verification

| Tool | Used for |
| ---- | -------- |
| Claude (Anthropic) | Draft of XML data, XSD, XSLT, OpenAPI specification; programme page filter and saved-events code; venue columns, mobile layout fix and test scripts in this version |
| Gemini / Nano Banana 2 | All images |

How outputs were verified:
- Data: record counts compared between XML and the programme page (60 / 60).
- Layout: opened in a real browser at several widths; page width measured, not judged by eye.
- Functions: every search and filter option and the save / reload / remove flow were run in a real browser and the numbers were counted.

What we accepted, changed or rejected:
- Rejected: third-party UI libraries (Bootstrap, Tailwind) proposed for the prototype; we kept plain HTML, CSS and JavaScript because the project must run without any installation.
- Changed: layout and spacing code needed manual correction because the generated output did not match what we intended (our own assessment in `readme.md`).
- Found by testing and corrected: the generated category dropdown listed 5 categories while the page uses 10 tags. We tested every option in the browser, counted the events per option and added the 3 missing categories.
- Found by measuring and corrected: the generated pages looked correct on a desktop but were wider than a phone screen. We measured page width at 320 and 375 px, changed the fixed grid minimum width and the non-wrapping heading, and measured again.
- Open issue, not fixed: in the Seminar 3 prototype and in `custom_list.html` the text of events is inserted with `innerHTML` and a template string. The text comes from our own data and from the page itself, not from visitors, so the risk is low, but we have not replaced it with `textContent`. The Seminar 3 documents said this had been secured; that statement was not correct and should not be repeated.
- Our own text was not accurate either: the Seminar 3 text described a combined search and filter and a custom list with the same layout as the programme; testing showed the code does not do exactly this (see shortcomings).

Limitations found: AI-generated pages passed a quick look on desktop but broke on a phone and had gaps in the filter; both were only found by measuring and testing.
