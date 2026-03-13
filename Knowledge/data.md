# MoMA Collection Dataset — Artworks.csv

Source: [github.com/MuseumofModernArt/collection](https://github.com/MuseumofModernArt/collection)

## Overview

| Metric | Value |
|--------|-------|
| Rows | 160,269 |
| Columns | 30 |
| File size | ~69 MB |
| Primary keys | `ObjectID` (numeric), `AccessionNumber` (text) — both unique |

---

## Columns

| Column | Type | Fill Rate | Unique | Description |
|--------|------|-----------|--------|-------------|
| **Title** | Text | 100.0% | 109,906 | Artwork title |
| **Date** | Text (free-form) | 98.7% | 8,515 | Creation date: "1896", "1976-77", "1920-1925", "c. 1950", "1960s", "n.d." |
| **Medium** | Text | 94.3% | 22,827 | Materials/technique, e.g. "Oil on canvas", "Gelatin silver print" |
| **Dimensions** | Text | 94.5% | 91,322 | Free-text imperial+metric, e.g. `19 1/8 x 66 1/2" (48.6 x 168.9 cm)` |
| **Classification** | Text | 100.0% | 42 | Artwork type: "Painting", "Print", "Architecture", "Photograph", etc. |
| **Department** | Categorical | 100.0% | 8 | MoMA department (see distribution below) |
| **CreditLine** | Text | 99.2% | 7,481 | Acquisition credit text |
| **AccessionNumber** | Text (PK) | 100.0% | 160,269 | Unique catalog number, e.g. "885.1996" |
| **ObjectID** | Integer (PK) | 100.0% | 160,269 | Numeric ID, range 2–507,396 |
| **Cataloged** | Boolean | 100.0% | 2 | Y (101,598) / N (58,671) |
| **DateAcquired** | Date (ISO) | 96.6% | 1,797 | YYYY-MM-DD format |
| **OnView** | Text | 0.6% | 70 | Gallery location if currently displayed |
| **Artist** | Text | 99.2% | 14,253 | Artist name(s), multi-artist delimited by ` \| ` |
| **ConstituentID** | Integer | 99.2% | 14,282 | Artist ID, range 1–141,100 |
| **ArtistBio** | Text | 95.8% | 8,997 | Parenthesized bio, e.g. "(Austrian, 1841–1918)" |
| **Nationality** | Text | 99.2% | 138 parsed | Parenthesized, e.g. "(French)". Multi-artist: `(American)(French)` |
| **BeginDate** | Integer (year) | 99.2% | 2,448 | Birth year. **0 = unknown.** Range: 0–2019 |
| **EndDate** | Integer (year) | 99.2% | 1,458 | Death year. **0 = living/unknown.** Range: 0–2026 |
| **Gender** | Text | 99.2% | 427 | Parenthesized: "(male)", "(female)", or multi-value |
| **URL** | URL | 63.4% | 101,598 | MoMA collection page |
| **ImageURL** | URL | 58.1% | 92,508 | Direct image link |
| **Height (cm)** | Float | 80.8% | 3,926 | Range: 0–9,140 |
| **Width (cm)** | Float | 80.2% | 3,974 | Range: 0–9,144 |
| **Depth (cm)** | Float | 11.6% | 1,422 | 3D objects only. Range: 0–1,808 |
| **Diameter (cm)** | Float | 0.9% | 508 | Circular objects. Range: 0.6–914 |
| **Length (cm)** | Float | 0.5% | 403 | Range: 0–8,321 |
| **Weight (kg)** | Float | 0.2% | 214 | Range: 0.09–185,068 |
| **Circumference (cm)** | Float | 0.0% (10) | 9 | Range: 9.9–83.8 |
| **Seat Height (cm)** | — | 0.0% | 0 | Completely empty |
| **Duration (sec.)** | Float | 1.2% | 696 | Time-based media. Range: 0–6,283,065 |

---

## Distributions

### By Department

| Department | Count | Share |
|------------|------:|------:|
| Drawings & Prints | 81,694 | 51.0% |
| Architecture & Design | 34,793 | 21.7% |
| Photography | 33,702 | 21.0% |
| Painting & Sculpture | 4,081 | 2.5% |
| Media and Performance | 3,267 | 2.0% |
| Fluxus Collection | 1,683 | 1.1% |
| Film | 1,018 | 0.6% |
| Architecture & Design - Image Archive | 31 | 0.0% |

### Top 11 Nationalities (parsed)

Multi-artist rows contribute to each artist's nationality separately.

| Nationality | Count |
|-------------|------:|
| American | 79,047 |
| French | 24,179 |
| German | 10,382 |
| British | 6,503 |
| Spanish | 3,457 |
| Italian | 3,375 |
| Japanese | 2,891 |
| Russian | 2,716 |
| Swiss | 2,533 |
| Dutch | 1,930 |
| Mexican | 1,783 |

### Top 10 Mediums (exact match)

| Medium | Count |
|--------|------:|
| Gelatin silver print | 17,056 |
| Lithograph | 8,762 |
| Pencil on paper | 7,317 |
| Albumen silver print | 4,841 |
| Pencil on tracing paper | 3,280 |
| Chromogenic print | 1,787 |
| Letterpress | 1,721 |
| Etching | 1,700 |
| Ink on paper | 1,573 |
| Oil on canvas | 1,069 |

### Broad Medium Categories (keyword grouping)

| Category | Count |
|----------|------:|
| Graphite/Pencil works | 26,310 |
| Lithographs | 23,718 |
| Gelatin silver prints | 22,015 |
| Etchings | 11,326 |
| Ink works | 7,420 |
| Woodcuts | 4,018 |
| Screenprints | 3,976 |
| Other photography | 3,461 |
| Video/Film | 2,939 |
| Offset lithographs | 1,446 |
| Oil on canvas | 1,190 |

---

## Data Quality Issues

- **Date is free-text.** Formats: single years ("1896"), abbreviated ranges ("1976-77"), full ranges ("1920-1925"), approximate ("c. 1950"), decades ("1960s"), descriptive ("Unknown", "n.d."). Requires custom parsing.
- **Parenthesized fields.** `Nationality`, `BeginDate`, `EndDate`, `Gender` are wrapped in parens and need stripping. Multi-artist rows concatenate without separators: `(American)(French)` — split with regex `\(([^)]*)\)`.
- **Multi-artist delimiter mismatch.** `Artist` uses ` | ` but `Nationality`/`Gender`/dates concatenate parens. Naive splitting (e.g. on `|`) misses multi-artist nationalities.
- **Dimensions are free-text.** Use the structured Height/Width/Depth columns instead when possible.
- **Sentinel value 0.** `BeginDate` and `EndDate` use 0 for unknown/living — filter when computing birth/death stats.
- **OnView quoting.** Values include wrapping quotes: `"MoMA, Floor 4, 417"`.
- **30,312 rows (18.9%) lack both Height and Width.** No URL exactly matches uncataloged: both are 58,671.
- **75% of the 22,827 unique medium strings appear exactly once** — long-tail free-text cataloging.
- **Near-empty columns:** Seat Height (0 rows), Circumference (10), Weight (300), Length (734) — only relevant for furniture/sculpture.

---

## Notable Patterns

1. **Works-on-paper dominance.** Over 70% of the collection is prints, drawings, and photographs. Paintings are only 2.5% — MoMA's holdings are far broader than its public image.
2. **Photography is the largest single medium family.** ~24% of all artworks, with gelatin silver prints alone (17,056) outnumbering any other exact medium.
3. **Cataloging gap.** 58,671 artworks (36.6%) have `Cataloged = N` and no URL/online presence.
