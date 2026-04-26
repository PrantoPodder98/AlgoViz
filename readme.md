# search_6.html — AlgoViz: Search Algorithms Visual Guide
## Summary for Context Preservation (upload this to a new chat to resume work)

---

## What This File Is

A **single-file HTML application** (≈2200 lines) that visualises 8 search/AI algorithms step-by-step with interactive controls. Built with vanilla HTML/CSS/JavaScript + HTML5 Canvas. No dependencies — open in any browser.

**Fonts:** Syne (headings/UI), JetBrains Mono (code/labels), Nunito (body)  
**Theme:** Light (white background, slate borders, coloured accents)

---

## Layout Structure

```
<nav>           ← Top tab bar (BFS | DFS | DFDB | Greedy | A* | TSP | 8-Puzzle | Decision Tree)
<div.layout>
  <div.vis-panel>        ← LEFT: Canvas visualisation area
    <div.vis-header>     ← Scenario tag + title + description
    <canvas id="c">      ← All algorithm drawings rendered here
  <div.step-panel>       ← RIGHT: Fixed-width (500px) panel
    <div.step-header>    ← "Step-by-Step Walkthrough" heading (sticky)
    <div.step-scroll>    ← SCROLLABLE — all content below
      #algoExplainer     ← Algorithm theory banner (hidden until algo loads)
      #stepCurrent       ← Big card showing current step title + explanation
      #stateCards        ← Small stat cards (e.g. Queue size, Depth, Cost)
      #legendArea        ← Colour legend dots
      #stepHistory       ← History of previous steps
    </div.step-scroll>
    <div.controls>       ← FIXED BOTTOM: Restart | ← Back | Next Step →
```

---

## CSS Variables (Light Theme)

```css
--bg: #f1f5f9          /* page background */
--surface: #ffffff     /* panel/card background */
--card: #f8fafc        /* nested card background */
--border: #e2e8f0      /* borders */
--border2: #cbd5e1     /* stronger borders */
--cyan: #0369a1        /* primary accent (nav, headers) */
--green: #15803d       /* success */
--orange: #c2410c      /* warning/current */
--red: #b91c1c         /* error/random route */
--purple: #6d28d9      /* path/2-opt */
--yellow: #b45309      /* heuristic labels */
--text: #0f172a        /* primary text */
--text2: #334155       /* secondary text */
--text3: #64748b       /* muted text */
```

---

## Canvas Drawing Primitives (global, used by all algos)

| Function | Purpose |
|---|---|
| `bg()` | Fill canvas #f8fafc + subtle grey grid lines |
| `circle(x,y,r,fill,stroke,lw)` | Draw a filled circle node |
| `label(text,x,y,size,color,align,baseline,bold)` | Draw text |
| `monoLabel(text,x,y,size,color,align)` | Monospace text |
| `line(x1,y1,x2,y2,color,lw)` | Straight line |
| `arrow(x1,y1,x2,y2,color,lw)` | Line with arrowhead |
| `pill(text,x,y,color,bg)` | Rounded pill label |
| `edgeLabel(text,x,y)` | Yellow pill (edge weights) |
| `roundRect(x,y,w,h,r,fill,stroke,lw)` | Rounded rectangle |
| `drawNode(x,y,r,lbl,state,hLabel)` | Full node with state colouring + optional h label |
| `drawEdgeLine(x1,y1,x2,y2,color,lw,wLabel)` | Edge with padding + optional weight label |

### Node States → Colours (Light Theme)
| State | Fill | Border |
|---|---|---|
| `start` | #dbeafe (light blue) | #2563eb |
| `goal` | #dcfce7 (light green) | #16a34a |
| `current` | #fed7aa (light orange) | #ea580c |
| `frontier` | #d1fae5 (mint green) | #059669 |
| `visited` | #e2e8f0 (grey) | #94a3b8 |
| `path` | #ede9fe (light purple) | #7c3aed |
| `idle` | #f1f5f9 (off-white) | #94a3b8 |

---

## Algorithm Modules

Each algorithm is an **IIFE** returning `{init, nextStep, prevStep, draw}`.  
All are registered in `ALGOS` map and wired to tabs.

### 1. BFS — Breadth-First Search (`BFS_ALGO`)
- **Tab:** `bfs`
- **Scenario:** 🚨 City Rescue Mission — Fire Station → Rescue Site
- **Graph:** 13 nodes, unweighted edges
- **Canvas:** Nodes + edges, queue visualiser at bottom (blue pills)
- **Steps:** Init → dequeue each node → found → path traceback with parent pointers
- **Key stat cards:** Visited count, Queue size, Depth, Added

### 2. DFS — Depth-First Search (`DFS_ALGO`)
- **Tab:** `dfs`
- **Scenario:** 🧭 Cave Network — Entrance → Treasure
- **Graph:** 13 nodes, unweighted
- **Canvas:** Nodes + edges, stack visualiser at bottom (orange pills)
- **Steps:** Init → pop/push each node → dead ends → path found (not optimal)

### 3. DFDB — Depth-Limited Search (`DFDB_ALGO`)
- **Tab:** `dfdb`
- **Scenario:** 🏢 Building Search — Reception → Child Found
- **Graph:** 12 nodes, depth limit = 3
- **Canvas:** Nodes + edges + red dashed vertical line at depth limit
- **Steps:** Explores normally but prunes nodes at depth ≥ limit

### 4. Greedy Best-First Search (`GREEDY_ALGO`)
- **Tab:** `greedy`
- **Scenario:** 🏥 Hospital Emergency — Ambulance Base → Hospital
- **Graph:** 12 nodes, weighted, each has h(n) heuristic value (gold label)
- **Canvas:** Nodes show h= label below; open list visualiser at bottom
- **Steps:** Always expands min-h node; warns this is NOT optimal

### 5. A\* Search (`ASTAR_ALGO`)
- **Tab:** `astar`
- **Scenario:** ✈ Airport Route — Start → Airport
- **Graph:** 12 nodes, weighted, each has h(n) heuristic
- **Canvas:** Nodes show h= (gold) and g= (blue) labels; open list with f=g+h values
- **Steps:** Expands min-f node; updates parents when cheaper path found; traceback shows parent chain

### 6. TSP — Travelling Salesperson (`TSP_ALGO`)
- **Tab:** `tsp`
- **Scenario:** 📦 Delivery Driver — Warehouse + 9 cities
- **Cities:** 10 fixed positions (Warehouse W, cities 1–9)
- **Canvas Features:**
  - **Always visible:** All possible city-to-city edges drawn as grey **dashed lines** with **distance values** shown at midpoints
  - **Active route:** Solid coloured lines drawn on top (red=random, blue=NNA, purple=2-opt)
  - **Newly added edge** during NNA: highlighted orange
  - Phase badge pill at top centre
  - Total distance shown at bottom
- **Phases:** intro → random → nna (step-by-step) → nna_done → twoopt_start → twoopt_done → summary
- **Key functions:**
  - `dist(a,b)` — Euclidean distance scaled to canvas
  - `tourCost(tour)` — Sum of all edge distances in tour
  - `nna()` — Nearest Neighbour heuristic
  - `twoOpt(tour)` — 2-opt local improvement

### 7. 8-Puzzle (`PUZZLE_ALGO`)
- **Tab:** `puzzle`
- **Scenario:** 🧩 Sliding Puzzle — BFS solver
- **Start:** `[1,2,3,4,0,6,7,5,8]` → Goal: `[1,2,3,4,5,6,7,8,0]`
- **Canvas:** 3×3 current board (left) + goal board (right) + progress bar
- **Tile colours:** light green = correct position, orange = just moved, white = unplaced
- **Steps:** Each BFS solution move shown one at a time

### 8. Decision Tree (`DECISION_ALGO`)
- **Tab:** `decision`
- **Scenario:** 🏦 Bank Loan Application — Sarah's credit decision
- **Tree:** 8+ nodes, yes/no branching (square = decision, circle = leaf outcome)
- **Canvas:** Tree revealed node-by-node; path edges green (YES) or red (NO)
- **Applicant profile card** shown top-right on canvas (credit score, income, debts, etc.)
- **Randomised applicant** on each Restart for variety

---

## Global UI Helper Functions

```javascript
setHeader(tag, title, desc)      // updates vis-header area
setAlgoExplainer(title, body)    // shows the algo theory banner
showStep(s)                      // renders current step card
setStateCards(cards)             // renders small stat cards [{val, lbl}]
setLegend(items)                 // renders legend [{color, label, sq?, border?}]
addHistory(s)                    // appends a step to history list
updateBtns(stepI, stepsLen)      // enables/disables Back/Next buttons
clearHistory()                   // clears history list
```

---

## Known Improvements Made in v6 (this file)

1. **Full light theme** — white canvas with subtle grid, all dark hex colours replaced with light equivalents throughout all drawing functions
2. **Fixed controls panel** — `.controls` moved outside `.step-scroll` div, uses `position:sticky; bottom:0` so buttons always visible; right panel content scrolls independently
3. **TSP dashed background edges** — All city-to-city connections always shown as dashed grey lines with distance labels before/during routing; active route drawn as solid coloured lines on top
4. **Summary doc** — this file

---

## How to Continue Work in a New Chat

1. Upload `search_6.html` and this file `search_6_summary.md`
2. Tell Claude: *"This is my AlgoViz app. Read the summary doc first, then read the HTML file for details."*
3. The summary saves ~1000 tokens of re-reading the full file for common tasks.

---

## Possible Future Improvements

- Add animation transitions between steps (CSS transitions on canvas via off-screen buffer)
- Add speed control slider for auto-play mode
- Make TSP show explicit swap animation for 2-opt (highlight the two edges being uncrossed)
- Add IDDFS (Iterative Deepening) as a 9th tab
- Mobile responsive layout (collapse right panel to bottom drawer)
- Add a "Compare" mode showing BFS vs DFS side-by-side
