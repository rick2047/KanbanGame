# Kanban Workflow Game — Proposal

## Why

Build a real-time kanban workflow management simulation game where players assign workers to tickets and manage the chaos of defects, rework loops, and delivery pressure. The game demonstrates how quality and process interact in software development through emergent gameplay.

## What Changes

- Create an HTML5 Canvas web game with separate files (`index.html`, `styles.css`, `script.js`)
- Implement a discrete-tick simulation engine (500ms ticks)
- Build object-oriented JavaScript architecture with GameState, Ticket, Worker, and Project classes
- Create kanban board UI with 5 columns: Backlog → Todo → In Progress → Testing → Done
- Implement drag-and-drop worker assignment system
- Add three interconnected rework loops (merged components, tester escapes, customer defects)
- Use binomial probability model for skill-based defect generation
- Deploy to GitHub Pages for easy access

## Capabilities

### New Capabilities

- `game-engine`: Core simulation loop, tick-based updates, game state management
- `kanban-board`: UI rendering, column layout, ticket visualization
- `worker-system`: Worker roles (BA/Dev/Tester), skill mechanics, assignment logic
- `ticket-lifecycle`: Ticket creation, status transitions, defect tracking
- `project-generation`: Project spawning, component decomposition, story point allocation
- `rework-mechanics`: Binomial defect sampling, rework loop triggers, customer anger system

### Modified Capabilities

- None (greenfield project)

## Impact

- New GitHub Pages site deployment
- Pure client-side JavaScript (no backend)
- Multi-file architecture (index.html, styles.css, script.js) for better organization
- Canvas-based rendering with DOM overlay for interactions
