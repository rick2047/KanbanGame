## ADDED Requirements

### Requirement: Fixed Spawn Cadence with Active Project Cap
The system SHALL attempt to spawn a new project every 60 seconds and SHALL NOT exceed four active projects at any time.

#### Scenario: Spawn blocked at cap
- **WHEN** the spawn timer elapses while four projects are active
- **THEN** no additional project is created

### Requirement: Generated Project Composition Bounds
Each generated project SHALL include a random component count from 1 to 10, and each component SHALL have integer story points from 1 to 8.

#### Scenario: Generated component values are in range
- **WHEN** a project is generated
- **THEN** component count is between 1 and 10 and each component point value is between 1 and 8

### Requirement: BA Decomposition Output
When a BA analyzes a project, the resulting component tickets SHALL be emitted to Todo as one batch from that analysis action.

#### Scenario: BA analysis creates todo batch
- **WHEN** BA analysis completes for a project
- **THEN** all resulting tickets from that project appear in Todo together
