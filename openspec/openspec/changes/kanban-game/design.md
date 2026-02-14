# Kanban Workflow Game — Design

## Context

We're building a kanban workflow simulation game that demonstrates how quality and process interact in software development. The game uses a binomial probability model to create emergent rework loops.

**Key constraints from exploration:**
- Discrete tick-based simulation (500ms ticks)
- Integer worker speeds only
- Object-oriented JavaScript
- HTML/CSS plus JavaScript ES modules
- GitHub Pages deployment
- pSkill limited to 0.5-1.0 range

## Goals / Non-Goals

**Goals:**
- Create a playable simulation that demonstrates kanban workflow dynamics
- Implement three rework loops (merged components, tester escapes, customer defects)
- Allow drag-and-drop worker assignment
- Provide real-time visual feedback of game state
- Deploy to GitHub Pages for easy sharing

**Non-Goals:**
- Multiplayer or networked play
- Save/load game state (for MVP)
- Sound effects or music
- Mobile-responsive design (desktop-first)
- AI/automation for worker assignment

## Decisions

### File Structure: HTML/CSS + JavaScript ES Modules
**Decision:** Use `index.html` and `styles.css` plus multiple JavaScript ES module files (e.g., `src/models/*.js`, `src/sim/*.js`, `src/ui/*.js`) loaded from a minimal entry module.

**Rationale:** Improves modularity, makes parallel development safer for multiple subagents, and reduces merge conflicts versus a single large `script.js`.

**Alternatives considered:** Single HTML file with embedded CSS/JS and one monolithic `script.js` (both rejected for maintainability and collaboration constraints).

### Game Loop: Discrete Ticks
**Decision:** Use 500ms discrete ticks with `setInterval`.

**Rationale:** Simplifies simulation logic, matches board game feel, easy to debug.

**Tick responsibilities:**
1. Orchestrate all assigned workers by calling `work()` once per tick
2. Apply any returned worker outcomes to shared state ordering (column arrays, counters)
3. Spawn new projects (every 120 ticks = 60 seconds)

### Ticket Transform Ownership
**Decision:** All ticket transformation logic SHALL live in `Worker.work()` (role-specific behavior).

**Rationale:** Keeps business rules with the role that performs the work, reducing split logic between orchestration and domain behavior.

### State Management: Centralized GameState
**Decision:** Single `GameState` class owns all mutable state.

**Rationale:** Predictable state flow, easy to serialize later if we add save/load.

```javascript
class GameState {
  columns = {
    backlog: [],
    todo: [],
    inProgress: [],
    testing: [],
    done: []
  };
  workers = [];
  projects = [];
  tickCount = 0;
  lastProjectSpawn = 0;
}
```

### Binomial Defect Model
**Decision:** When work completes, roll per-point binomial for defects.

**Rationale:** Simple to implement, matches PRD exactly.

```javascript
function resolveWorkQuality(pointsWorked, pSkill) {
  let correctPoints = 0;
  for (let i = 0; i < pointsWorked; i++) {
    if (Math.random() < pSkill) correctPoints++;
  }
  return {
    correctPoints,
    defectPoints: pointsWorked - correctPoints
  };
}
```

This quality resolution is invoked from `Worker.work()` when the role reaches completion criteria for its current ticket.

### Worker Assignment: One Worker Per Ticket
**Decision:** Workers are exclusively assigned to one ticket until completion.

**Rationale:** Simplifies logic, clear player decision space, matches real-world kanban.

### Drag-and-Drop: HTML5 Native API
**Decision:** Use HTML5 Drag and Drop API for worker assignment.

**Rationale:** Native browser support, works with canvas and DOM elements.

### Rendering: DOM with CSS Styling
**Decision:** Use DOM elements for the entire UI with CSS for styling. No Canvas.

**Rationale:** CSS provides better styling control, easier to implement responsive layouts, standard drag-and-drop works naturally with DOM elements.

### Project Spawning: Fixed Interval
**Decision:** New project every 120 ticks (60 seconds).

**Rationale:** Predictable pressure, easy to tune later.

### Project Generation Algorithm
**Decision:** 
1. Random component count (1-10)
2. Random story points per component (1-8)
3. Store components array on project

**Rationale:** Creates varied project sizes, simple to implement.

## Architecture

```
┌─────────────────────────────────────────┐
│           index.html                    │
│  ┌───────────────────────────────────┐  │
│  │  Board (DOM)                      │  │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ │  │
│  │  │Backlog │ │  Todo  │ │ In     │ │  │
│  │  │        │ │        │ │ Prog   │ │  │
│  │  └────────┘ └────────┘ └────────┘ │  │
│  └───────────────────────────────────┘  │
│  ┌──────────────┐  ┌──────────────┐     │
│  │    Stats     │  │ Drag-drop    │     │
│  │   (DOM)      │  │  handlers    │     │
│  └──────────────┘  └──────────────┘     │
│  ┌──────────────┐                       │
│  │  Worker      │                       │
│  │  Reserve     │                       │
│  │  (DOM)       │                       │
│  └──────────────┘                       │
└─────────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────┐
│      src/main.js + ES modules           │
│  ┌─────────────┐ ┌───────────────────┐  │
│  │ src/sim/*   │ │   src/ui/*        │  │
│  │ game loop   │ │ render + dnd      │  │
│  └─────────────┘ └───────────────────┘  │
│  ┌────────────────────────────────────┐ │
│  │ src/models/* (GameState/Ticket/...)│ │
│  └────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

## Class Hierarchy

```javascript
class Game {
  constructor() // init state, loop, renderer
  start()
  tick() // main update loop
}

class GameState {
  columns: Object<ColumnName, Ticket[]>
  workers: Worker[]
  projects: Project[]
  tickCount: number
  
  addTicket(ticket, column)
  moveTicket(ticket, fromColumn, toColumn)
  removeTicket(ticket, column)
}

class Project {
  id: string
  components: Component[] // {name, points}
  totalPoints: number
}

class Ticket {
  id: string
  projectId: string
  components: Component[] // can be 1+ (merged)
  points: number
  defectPoints: number
  progress: number // work completed (0 to points)
  assignee: Worker|null
  status: ColumnName
  
  isOversized() // components.length > 1
  addProgress(amount) // returns true if completed
}

class Worker {
  id: string
  name: string
  role: 'BA'|'Dev'|'Tester'
  pSkill: number // 0.5 to 1.0
  speed: number // integer points per tick
  ticket: Ticket|null
  
  assign(ticket)
  unassign()
  work() // called each tick; applies role logic, transitions, and ticket mutations
}

class GameRenderer {
  constructor(container, state)
  render() // render board, columns, tickets to DOM
  renderTicket(ticket) // creates/updates DOM element
  renderWorker(worker) // creates/updates DOM element
  clear() // remove all rendered elements
}
```

## State Transitions

```
Project Spawn
└──► Backlog (as unanalyzed project)
    └──► BA analyzes (instant, creates tickets)
        └──► Todo (component tickets)
            └──► Dev assigned → In Progress
                ├──► Work completes with defects
                │   └──► Testing
                │       └──► Tester assigned
                │           ├──► Defects found
                │           │   └──► Rework ticket → Todo
                │           └──► No defects or done
                │               └──► Done
                │                   └──► If defectPoints > 0
                │                       └──► Customer angry → Todo
                └──► Oversized detected (components.length > 1)
                    └──► Backlog (wasted effort)
```

## Risks / Trade-offs

**[Risk] Performance with many tickets** → Mitigation: Limit max projects active (max 4 concurrent projects), use object pooling if needed (future optimization).

**[Risk] CSS styling complexity** → Mitigation: Use CSS for all styling, keep visual design simple.

**[Risk] Drag-drop API browser compatibility** → Mitigation: Standard API, well supported. Can fall back to click-to-assign if needed.

**[Risk] Game balance (too easy/hard)** → Mitigation: Tuneable constants (spawn rate, worker skills), adjust after playtesting.

**[Trade-off] No save/load in MVP** → Players lose progress on refresh. Acceptable for v1.

## Implementation Order

1. Core classes (GameState, Ticket, Worker, Project)
2. Game loop with tick mechanism
3. DOM rendering (board, columns, placeholder tickets)
4. CSS styling
5. Worker assignment (drag-drop)
6. Work simulation (progress accumulation)
7. Defect creation and rework loops
8. Project spawning
9. UI polish (stats, indicators)

## Constants

```javascript
const TICK_MS = 500;
const PROJECT_SPAWN_TICKS = 120; // 60 seconds
const MIN_COMPONENTS = 1;
const MAX_COMPONENTS = 10;
const MIN_POINTS = 1;
const MAX_POINTS = 8;
const MIN_P_SKILL = 0.5;
const MAX_P_SKILL = 1.0;
const MAX_ACTIVE_PROJECTS = 4;
```
