# LineageOS — ChatGPT Handoff Document

## What This Is

You (ChatGPT) have been running ancestry/genealogy research for Elliot Goldman. This document hands off a **working prototype** of **LineageOS** — a venture-demo-quality web app that visualizes a global human ancestry graph. The prototype is built. Your job now is to **integrate the real research you've found into this codebase** and help make it a functioning product people can use today.

The full source code is in a single file: **`lineageos.html`** in this GitHub repo.

---

## The Prototype — What's Already Built

A complete single-page web app (one self-contained HTML file, ~700 lines) with these sections:

### 1. Hero / Landing Page
- Animated particle network background (canvas)
- Stats bar with animated counters
- "Every Human. One Family Tree." headline

### 2. Lineage Explorer (Interactive)
- **Search bar** — type any name to find people in the database
- **Profile cards** — name, birth/death, location, confidence score
- **Interactive family tree** — canvas-rendered, zoomable, pannable, shows 6 generations
- ~60 pre-seeded people with parent relationships

### 3. Relationship Engine
- "How am I related to Abraham Lincoln?" style queries
- Visual path display (A → B → C → ... → Target)
- Pre-built demo path: Elliot Goldman → Abraham Lincoln (9 steps)

### 4. Evidence Layer
- Click any relationship node to see evidence sources
- Sources: birth certificates, census records, DNA matches, ship manifests
- Each source has its own confidence contribution

### 5. Timeline View
- Vertical timeline from present (1997) back to 1700s
- Confidence fading (opacity decreases) as you go further back
- Scroll-triggered animations

### 6. DNA Layer
- Donut chart: 45% Ashkenazi Jewish, 28% Eastern European, 12% Central European, 8% Mediterranean, 5% Middle Eastern, 2% Other
- Haplogroups: J-M267 (paternal), K1a1b1a (maternal)
- Neanderthal DNA: 2.4%
- Shared ancestors estimate: ~12,400

### 7. Global Migration Map
- Stylized SVG world map with continent outlines
- Animated migration path: Levant (500 CE) → Iberia (900) → Central Europe (1200) → Poland (1500) → Lodz (1850) → New York (1928)
- Timeline slider to scrub through centuries

### 8. AI Prediction Panel
- 9 ancestor cards with status: ✓ Confirmed / ◐ Likely / ○ Hypothetical
- Confidence bars for each prediction

### 9. Architecture & Vision
- Data pipeline diagram (Sources → Ingestion → Graph → Intelligence → App)
- Roadmap through 2028

---

## Data Architecture (Current)

The data model is a simple JavaScript object array. Each person has:

```javascript
{
  id: Number,
  name: String,
  birth: Number (year),
  death: Number | null,
  location: String,
  confidence: Number (0-100),
  fatherId: Number | null,
  motherId: Number | null,
  gen: Number (generation relative to Elliot)
}
```

People are stored in `PEOPLE` array and `PERSON_MAP` (id → person) for O(1) lookup. The tree is built by traversing `fatherId`/`motherId` pointers recursively.

### Pre-seeded Family Tree for Elliot Goldman

```
Elliot Goldman (b. 1997, New York)
├── Father: David Goldman (b. 1965, Brooklyn)
│   ├── Samuel Goldman (1935-2010, Brooklyn)
│   │   ├── Abraham Goldman (1905-1978, Lodz → Brooklyn)
│   │   │   ├── Moshe Goldman (1875-1945, Lodz)
│   │   │   │   ├── Tzvi Goldman (1845-1920, Lodz)
│   │   │   │   └── Rivka Goldschmidt (1848-1925, Lodz)
│   │   │   └── Chaya Birnbaum (1878-1942, Lodz)
│   │   └── Miriam Rosen (1908-1985, Warsaw → Brooklyn)
│   │       ├── Yosef Rosen (1880-1943, Warsaw)
│   │       └── Freida Shapiro (1882-1944, Warsaw)
│   └── Ruth Levine (1938-2018, Queens)
│       ├── Isaac Levine (1910-1990, Vilnius → Queens)
│       │   ├── Eliezer Levine (1878-1940, Vilnius)
│       │   └── Dvora Margolis (1882-1941, Vilnius)
│       └── Hannah Stern (1912-1992, Minsk → Queens)
└── Mother: Sarah Katz (b. 1968, Manhattan)
    ├── Morris Katz (1940-2015, Philadelphia)
    │   ├── Jacob Katz (1912-1980, Krakow → Philadelphia)
    │   └── Leah Friedman (1915-1988, Budapest → Philadelphia)
    └── Esther Weinberg (b. 1942, Philadelphia)
        ├── Nathan Weinberg (1914-1985, Vienna → Philadelphia)
        └── Rosa Blum (1918-1990, Prague → Philadelphia)
```

**Siblings:** Rebecca Goldman (b. 2000)
**Uncle:** Joshua Goldman (b. 1962, Boston) → cousin Amy Goldman (b. 1990)

---

## Your Mission: Integrate Research & Make It Real

### Phase 1: Inject Your Research Data

You've been researching Elliot's ancestry. Take everything you've found and:

1. **Replace fake data with real data** — any actual names, dates, locations, immigration records, census hits, etc. you've found should replace the placeholder data in the `PEOPLE` array and the tree structure.

2. **Add new people** — if you found additional ancestors, relatives, or connections not in the tree above, add them using the `addPerson()` function and link them via `fatherId`/`motherId`.

3. **Update confidence scores** — set confidence based on your evidence:
   - 95-100%: Multiple corroborating official records
   - 80-94%: Census + one other source
   - 60-79%: Single source or family tree agreement
   - 40-59%: AI-predicted / inferred
   - <40%: Hypothetical

4. **Update the DNA section** — if you have more specific genetic ancestry data, update the percentages and haplogroup information.

5. **Update the migration path** — adjust waypoints in the `migPath` array to match actual known migration history.

6. **Update the timeline** — the `ancestors` array in the timeline section should reflect the real ancestral line.

7. **Update AI predictions** — move confirmed ancestors from "Predicted" to "Confirmed" and add new predictions.

### Phase 2: Make It Production-Ready

To turn this from a demo into something people can actually use TODAY:

#### A. Add a Real Backend (Suggested: Supabase or Firebase)

```
1. Create a Supabase project
2. Create a `people` table matching the data model above
3. Create a `relationships` table (person_a_id, person_b_id, type, confidence, evidence_json)
4. Add API calls to fetch/search people
5. Add user auth so people can add their own family members
```

#### B. Enable User-Contributed Data

- Add an "Add Person" form (name, birth, death, location, parent links)
- Add an "Edit" button on profile cards
- Store user submissions in the database
- Flag user-contributed vs. verified data

#### C. Connect to Real Data Sources

The architecture page lists these — here's how to wire the first few:

| Source | API/Method | What It Provides |
|--------|-----------|-----------------|
| FamilySearch | familysearch.org/developers | GEDCOM trees, historical records |
| OpenCage Geocoding | opencagedata.com | Location → coordinates for the map |
| GEDmatch | Manual upload | DNA segment matching |
| JewishGen | jewishgen.org | Holocaust records, vital records |
| Ancestry (unofficial) | Web scraping / manual | Tree data |

#### D. Make the Map Real

Replace the simplified SVG continents with a real map library:
- **Leaflet.js** (lightweight, free) or **Mapbox GL** (prettier, has a free tier)
- Plot actual coordinates for each ancestor's location
- Draw migration arcs between locations

#### E. Relationship Pathfinding

Currently hardcoded for the Lincoln demo. To make it real:
- Implement BFS/Dijkstra on the person graph
- Find shortest path between any two people via parent links
- Calculate relationship degree (cousin, removed, etc.)

### Phase 3: Ship It

1. **Deploy to Vercel/Netlify** — the single HTML file works as-is, or split into a proper app
2. **Add a domain** — lineageos.com or similar
3. **Add sharing** — let people share their tree view via URL
4. **Add GEDCOM import** — let users upload .ged files to populate their tree
5. **Add invite flow** — "Invite family members to fill in gaps"

---

## Design System (Preserve These)

```css
Colors:
  --gold: #C9A84C (primary accent)
  --gold-light: #E8D5A0
  --gold-dark: #A08030
  --bg: #000 (background)
  --bg-card: rgba(255,255,255,0.03)
  --border: rgba(255,255,255,0.08)
  --border-gold: rgba(201,168,76,0.3)

Typography: Inter (Google Fonts)
  - Weights: 300, 400, 500, 600, 700, 800, 900
  - Headlines: 700-800 weight, tight letter-spacing (-1px to -2px)
  - Body: 400-500 weight

Confidence color coding:
  - High (90%+): green (#4cc94c)
  - Medium (50-89%): gold (#C9A84C)
  - Low (<50%): red (#c94c4c) or dim gray

Border radius: 12px (cards), 20px (large panels), 100px (pills/buttons)
```

---

## How to Edit the Code

The entire app is one HTML file. Key sections to find:

| Section | Search for this comment | What to edit |
|---------|------------------------|-------------|
| People database | `// DATA` | Add/modify people in `PEOPLE` array |
| Family tree links | `elliot.fatherId=` | Parent-child relationships |
| Extra people | `extraNames` | The searchable people list |
| Hero animation | `// HERO CANVAS` | Particle network on landing page |
| Tree rendering | `function drawTree()` | How the family tree is drawn |
| Relationship paths | `function findRelationship()` | The relationship path logic |
| Evidence sources | `function showEvidence()` | Evidence modal content |
| Timeline data | `var ancestors=[` | Timeline entries |
| DNA data | `var data=[` (inside DNA section) | Genetic percentages |
| Haplogroups | `haplo-grid` | Haplogroup cards |
| Migration waypoints | `var migPath=[` | Map migration points |
| AI predictions | `var preds=[` | Prediction cards |

---

## GitHub Repo Structure

```
lineageos/
├── lineageos.html          # The complete app (start here)
├── CHATGPT_HANDOFF.md      # This document
├── README.md               # Public-facing README
└── (future)
    ├── api/                # Backend when ready
    ├── data/               # GEDCOM imports, seed data
    └── docs/               # Additional documentation
```

---

## Key Constraints

1. **All fake data** — nothing in the current prototype is real. Your research is the real data. Replace it.
2. **Single file for now** — keep it as one HTML file until there's a reason to split (backend, build system)
3. **Preserve the design** — the black/gold/white aesthetic is the brand. Don't change it.
4. **Confidence is king** — every data point needs a confidence score. This is what distinguishes LineageOS from a static family tree.
5. **Privacy** — real people's data should be handled carefully. Don't expose sensitive info publicly without consent.

---

## What To Send Back

After you integrate the research, send back:
1. The updated `lineageos.html` file (or a diff/patch of changes)
2. A summary of what real data you integrated
3. Any gaps or uncertainties you found
4. Recommended next steps for the product

Let's build the complete human family tree.
