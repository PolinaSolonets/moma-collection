# Visual Design — MoMA Collection Explorer

## Design Direction

Inspired by MoMA's own visual identity: high-contrast, typographically driven, generous whitespace. The interface should feel like a museum catalog, not a dashboard. Content first — chrome recedes.

---

## Typography

- **Headings and UI labels**: system sans-serif stack — `-apple-system, "Segoe UI", Helvetica, Arial, sans-serif`. Clean, neutral, stays out of the way.
- **Data cells (Title, Medium, Dimensions)**: same sans-serif at smaller size. No monospace for data — this isn't a spreadsheet.
- **Font sizes**: 14px base for table body, 13px for secondary text (dates, dimensions), 24px for page title, 12px for filter labels.
- **Weight**: 400 normal for body, 600 semibold for column headers and filter labels. No bold in data cells.
- **Letter-spacing**: +0.02em on uppercase labels (e.g. "DEPARTMENT", "CLASSIFICATION") for readability.

## Color

Minimal palette. Almost monochrome with one functional accent.

| Role | Value | Usage |
|------|-------|-------|
| Background | `#ffffff` | Page background |
| Surface | `#f7f7f7` | Filter bar, stats panel, alternate table rows |
| Border | `#e0e0e0` | Table lines, input borders, dividers |
| Text primary | `#1a1a1a` | Titles, artist names, headings |
| Text secondary | `#595959` | Dates, dimensions, metadata |
| Text tertiary | `#999999` | Placeholders, empty-state messages |
| Accent | `#0066cc` | Links, active sort indicator, selected filter |
| Accent hover | `#004499` | Link/button hover state |
| Focus ring | `#0066cc` | Solid 2px outline + 2px offset on focused inputs |

No colored backgrounds for filters or buttons. Borders and subtle weight changes signal interactivity.

## Layout

- **Max width**: 1200px, centered. No full-bleed — content shouldn't stretch across ultrawide monitors.
- **Filter bar**: horizontal row pinned below the page title. Search input spans full width on its own row; dropdowns and year inputs sit below in a flex row with 12px gaps.
- **Results table**: full width within the max-width container. No side panel — the table is the primary content.
- **Stats line**: single row above the table showing "Showing 342 of 160,269 artworks · 87 artists · 1901–1968". Compact, not a separate panel.
- **Detail modal**: overlay with white card (max-width 640px), centered vertically, semi-transparent backdrop (`rgba(0,0,0,0.5)`).
- **Pagination**: right-aligned below the table. Simple text: "← Prev  Page 3 of 47  Next →".

## Table Design

- **No outer border.** Top header separated by a 2px `#1a1a1a` line (heavy rule, museum-catalog style). Rows separated by 1px `#e0e0e0`.
- **Alternate row shading**: every other row `#f7f7f7`. Hover: `#f0f0f0`.
- **Column widths**: Thumbnail fixed 52px (40px image + cell padding). Title flexible (takes remaining space). Artist ~160px. Date ~72px. Medium ~180px. Classification ~100px. Department ~120px. Total fixed: ~684px, leaving ~516px for Title at max width.
- **Thumbnails**: 40×40px, `object-fit: cover`, with 2px border-radius. Missing images: light gray `#e0e0e0` square with no icon.
- **Sort indicator**: small arrow (▲/▼) next to the active column header, in accent color.
- **Text overflow**: Title and Medium truncate with ellipsis. Full text visible in the detail modal.

## Filter Bar

- **Inputs**: 36px height, 1px border `#e0e0e0`, 4px border-radius. No drop shadows.
- **Search input**: placeholder text "Search titles, artists, mediums…" in tertiary color.
- **Dropdowns**: native `<select>` elements — no custom dropdown UI. Consistent with the system, zero JS overhead.
- **Year inputs**: `type="number"`, 80px wide, labeled "From" and "To" in 12px uppercase.
- **Export button**: text-style button, no fill — just accent-colored text with underline on hover. Sits at the right end of the filter bar.

## Detail Modal

- **Backdrop**: click-to-dismiss.
- **Card**: white, 24px padding, 8px border-radius, subtle `box-shadow: 0 4px 24px rgba(0,0,0,0.15)`.
- **Image**: top of card, max-height 400px, `object-fit: contain`, centered. Gray background behind it if the image doesn't fill the space.
- **Fields**: listed vertically as label–value pairs. Label in semibold 12px uppercase tertiary text. Value in 14px primary text. 8px gap between pairs.
- **MoMA link**: at the bottom, accent color, opens in new tab.
- **Close**: "×" button top-right, 32×32px hit target, tertiary text, darkens on hover.

## Spacing System

Multiples of 4px: 4, 8, 12, 16, 24, 32, 48.

- Page padding: 24px horizontal, 16px vertical
- Between filter bar and table: 16px
- Table cell padding: 8px 12px
- Modal card padding: 24px
- Between filter controls: 12px

## Responsive Behavior

- **Below 900px**: filter dropdowns stack vertically (1 per row). Table hides Classification and Department columns. Thumbnails shrink to 32px.
- **Below 480px**: table shows only Thumbnail, Title, and Artist. Other fields visible in detail modal only.

## Empty & Loading States

- **Loading**: centered text "Loading 160,269 artworks…" in secondary color, with a subtle CSS animation (pulsing opacity). No spinner. Filter bar and table hidden until data is ready.
- **No results**: centered in the table area — "No artworks match your filters." in secondary color, 16px. Below it in tertiary: "Try broadening your search or removing a filter."

## Visual References

The design draws from:
- MoMA's own collection pages (moma.org/collection) — clean grid, minimal chrome
- Museum exhibition catalogs — heavy top rules, restrained typography
- Swiss/International Typographic Style — grid-based, content-driven hierarchy
