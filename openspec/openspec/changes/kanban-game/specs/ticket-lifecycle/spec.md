## ADDED Requirements

### Requirement: Ticket Project Provenance
Every ticket SHALL store a project identifier linking it to its parent project.

#### Scenario: Ticket keeps project id
- **WHEN** any ticket is created
- **THEN** the ticket includes a valid projectId value

### Requirement: Ticket Progress Tracking
Each ticket SHALL track progress on the ticket object itself and SHALL advance only through assigned worker processing.

#### Scenario: Progress updates on ticket
- **WHEN** an assigned worker completes a simulation tick
- **THEN** the ticket progress value increases according to worker speed and rules

### Requirement: Workflow Status Transitions
Tickets SHALL transition only through valid workflow moves: Backlog -> Todo -> In Progress -> Testing -> Done, with allowed rework returns defined by rework mechanics and initiated by role-specific worker `work()` outcomes.

#### Scenario: Valid transition accepted
- **WHEN** ticket processing completes a normal stage handoff
- **THEN** the ticket status updates to the next valid workflow column from worker-produced transition intent
