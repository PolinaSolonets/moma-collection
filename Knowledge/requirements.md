# MoMA Collection Research Interface — Requirements

A static single-page web application (`index.html`) for exploring the MoMA Artworks dataset. Built with vanilla JavaScript and CSS — no frameworks, no build step, no backend.

---

## Constraints

- **Single file**: all HTML, CSS, and JS in one `index.html`
- **Data loading**: reads a preprocessed JSON file (derived from Artworks.csv) at startup
- **Static hosting**: works from any static file server or `file://`
- **Performance target**: usable with 160k records — use pagination, avoid rendering all rows at once
- **No external dependencies**: no CDN libraries, no frameworks

---

## User Stories

### US-1: Search artworks by text

> As a researcher, I want to type a query into a search box and see artworks whose **Title**, **Artist**, or **Medium** contain my text, so I can quickly find specific works.

**Acceptance criteria:**
- Single search input at the top of the page
- Searches across Title, Artist, and Medium fields simultaneously
- Case-insensitive, substring matching
- Results update as the user types (debounced ~300ms)
- Display result count (e.g. "342 of 160,269 artworks")

---

### US-2: Filter by department, classification, and nationality

> As a researcher, I want to narrow results using dropdown filters for Department, Classification, and Nationality, so I can focus on a specific segment of the collection.

**Acceptance criteria:**
- Three dropdown/select filters, each populated from distinct values in the dataset
- Filters combine with AND logic (with each other and with search)
- An "All" option resets that filter
- Nationality values are cleaned (parentheses stripped, multi-artist values split)

---

### US-3: Filter by date range

> As a researcher, I want to set a start year and end year to see only artworks created within that period.

**Acceptance criteria:**
- Two numeric inputs: "From year" and "To year"
- Parses the free-text Date column to extract the earliest year (handles "1896", "1976-77", "1920-1925", "c. 1950", "1960s")
- Artworks with unparseable or missing dates are excluded when a date filter is active
- Combines with all other filters via AND logic

---

### US-4: Browse results in a paginated table

> As a researcher, I want to see matching artworks in a sortable table so I can scan and compare them.

**Acceptance criteria:**
- Table columns: Thumbnail (from ImageURL), Title, Artist, Date, Medium, Classification, Department
- Paginated — 50 rows per page with prev/next controls and page indicator
- Click any column header to sort ascending/descending
- Thumbnail shows a small image if ImageURL exists, or a placeholder if not
- Rows without a title display "(Untitled)"

---

### US-5: View artwork detail

> As a researcher, I want to click on an artwork row to see its full details in a side panel or modal.

**Acceptance criteria:**
- Shows all non-empty fields for the selected artwork
- Displays the image (from ImageURL) at a larger size if available
- Links to the MoMA collection page (URL field) if available, opening in a new tab
- Shows physical measurements (Height, Width, Depth, etc.) when present
- Close button or click-outside to dismiss

---

### US-6: Summary statistics

> As a researcher, I want to see at-a-glance statistics about the current filtered result set.

**Acceptance criteria:**
- Stats panel updates live as filters change
- Shows: total artworks count, number of unique artists, date range spanned

---

### US-7: Export filtered results

> As a researcher, I want to export my current filtered result set as a CSV file so I can analyze it in other tools.

**Acceptance criteria:**
- "Export CSV" button
- Downloads a CSV containing all columns for the currently filtered artworks
- Filename: `moma-export.csv`
- Works client-side using Blob + download link

---

## Data Preprocessing

Since the raw CSV is 69 MB, a preprocessing step converts it to a compact JSON file:

- Strip BOM and parse CSV
- Clean parenthesized fields (Nationality, Gender, BeginDate, EndDate)
- Parse the Date column to extract a numeric `yearMin` (earliest year) for filtering
- Split multi-artist rows into arrays
- Drop columns with near-zero fill rate: `Circumference (cm)`, `Seat Height (cm)`, `Length (cm)`, `Weight (kg)`
- Drop `ConstituentID` and `ArtistBio` (not needed for the interface)
- Output as `artworks.json` with compact field names — target < 30 MB

---

## Non-Goals

- No server-side search or API
- No user accounts or saved searches
- No editing or write-back to the dataset
- No map visualization (nationality data is not geocoded)
- No image gallery / grid view (out of scope for v1)
- No charts or visualizations (v2 candidate)
- No timeline / decade histogram (v2 candidate)
