## ADDED Requirements

### Requirement: Five Workflow Columns
The UI SHALL present the workflow as five columns in order: Backlog, Todo, In Progress, Testing, and Done.

#### Scenario: Board renders workflow columns
- **WHEN** the game view loads
- **THEN** all five columns are visible in the defined order

### Requirement: DOM-Based Board Rendering
The UI SHALL render board and ticket elements using DOM elements and CSS styling.

#### Scenario: Board uses DOM and CSS
- **WHEN** the board is rendered
- **THEN** columns and tickets are represented as styled DOM elements

### Requirement: Worker Assignment by Drag and Drop
The UI SHALL allow assigning an available worker by dragging a worker card from the reserve and dropping it on a compatible ticket.

#### Scenario: Successful drag-and-drop assignment
- **WHEN** the player drops an eligible worker onto an eligible ticket
- **THEN** the worker becomes assigned to that ticket and is removed from the idle reserve display
