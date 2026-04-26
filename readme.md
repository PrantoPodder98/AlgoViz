## Overview

[index.html](index.html) is a single-file interactive web app for visual learning of search and AI algorithms. It is built with plain HTML, CSS, and JavaScript using Canvas for rendering.

## What the File Does

- Shows algorithm visualizations in a tab-based interface.
- Lets you step through each algorithm state using controls.
- Displays explanations, current step details, and history.
- Updates node and edge states visually while traversal or decision progresses.

## Algorithms Included in index.html

- Breadth-First Search (BFS)
- Depth-First Search (DFS)
- Depth-Limited Search (DFDB / DLS)
- Greedy Best-First Search
- A* Search
- Travelling Salesperson Problem (TSP) heuristic flow
- 8-Puzzle solver walkthrough
- Decision Tree path simulation

## UI Structure (inside index.html)

- Top navigation tabs for selecting algorithms.
- Left panel with canvas-based visualization.
- Right panel with:
  - current step explanation,
  - state cards,
  - legend,
  - step history,
  - control buttons (restart, previous, next).

## How to Run index.html

No installation is required.

1. Open [index.html](index.html) directly in a browser.

Optional local server:

```bash
cd "Intelligent System"
python3 -m http.server 8000
```

Then open:

- http://localhost:8000/index.html

## Technical Notes

- Framework-free implementation (vanilla JavaScript).
- Uses HTML5 Canvas drawing utilities for nodes, edges, labels, and overlays.
- Algorithm modules are organized in script sections and switched by active tab.
- Designed as a learning and demo tool.

## Intended Use

- Exam revision
- Classroom demonstrations
- Self-study for understanding algorithm behavior step by step
