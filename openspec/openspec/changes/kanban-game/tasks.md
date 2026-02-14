## 1. Foundation and File Scaffolding

- [ ] 1.1 Create `index.html` and `styles.css`, plus JavaScript module entrypoint (`src/main.js`) wired via `<script type="module">`
- [ ] 1.2 Add top-level DOM regions for board columns, worker reserve, and HUD/stats placeholders in `index.html`
- [ ] 1.3 Scaffold module folders/files (`src/models`, `src/sim`, `src/ui`) and split responsibilities to reduce merge conflicts between subagents

## 2. Parallel Wave A - UI Shell (Subagent Lane A)

- [ ] 2.1 Implement five workflow columns in DOM order: Backlog, Todo, In Progress, Testing, Done
- [ ] 2.2 Implement worker reserve container and worker card markup hooks for drag-and-drop
- [ ] 2.3 Implement base card markup hooks for tickets, assignment chips, progress text, and status labels (without exposing hidden defect/oversized signals)

## 3. Parallel Wave B - Styling System (Subagent Lane B)

- [ ] 3.1 Implement board and column layout styling in `styles.css` for desktop-first playability
- [ ] 3.2 Implement ticket and worker card styles, including clear role differentiation and readable status/progress presentation
- [ ] 3.3 Implement drag state and drop-target affordances in CSS without revealing hidden internal quality state

## 4. Parallel Wave C - Domain and Simulation Core (Subagent Lane C)

- [ ] 4.1 Implement `GameState`, `Project`, `Ticket`, and `Worker` classes with required fields and constraints (`pSkill` 0.5-1.0, integer speed)
- [ ] 4.2 Implement role+number starter roster initialization (`BA-1`, `Dev-1`, `Dev-2`, `Tester-1`) and exclusive assignment enforcement
- [ ] 4.3 Implement 500 ms game loop orchestration that calls each assigned worker `work()` once per tick and runs spawn checks
- [ ] 4.4 Implement worker-owned ticket transformations in `work()` for BA, Dev, and Tester role flows

## 5. Integration Wave - Workflow and Rework Mechanics

- [ ] 5.1 Implement fixed project spawning (every 60s) with max 4 active projects and bounded random generation (1-10 components, 1-8 points each)
- [ ] 5.2 Implement BA decomposition output to Todo as a batch with project provenance on all created tickets
- [ ] 5.3 Implement Dev flow including oversized discovery return to Backlog and quality resolution on completion
- [ ] 5.4 Implement Tester detection loop (partial defect detection creates Todo rework; remaining defects stay on original ticket)
- [ ] 5.5 Implement Done/customer anger loop to create Todo rework equal to remaining defect points

## 6. UI-Simulation Binding and Manual Validation

- [ ] 6.1 Bind renderer updates to `GameState` so DOM reflects column transitions, assignments, and progress each tick
- [ ] 6.2 Implement drag-and-drop assignment validation for compatible role/ticket combinations and reject invalid drops cleanly
- [ ] 6.3 Perform manual smoke run covering project spawn, assignment, in-progress transitions, testing outcomes, and customer rework loop
- [ ] 6.4 Verify hidden-state rules manually: no explicit UI exposure of defect points or oversized status before role-specific discovery
