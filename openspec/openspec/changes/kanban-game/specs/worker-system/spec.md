## ADDED Requirements

### Requirement: Starter Team Composition
The system SHALL initialize each new game with exactly four workers: BA-1, Dev-1, Dev-2, and Tester-1.

#### Scenario: New game worker roster
- **WHEN** a new game starts
- **THEN** the roster includes one BA, two Developers, and one Tester with role+number naming

### Requirement: Worker Attribute Constraints
Each worker SHALL have a role, an integer speed, and a pSkill value constrained to the range 0.5 through 1.0 inclusive.

#### Scenario: Worker attributes are valid
- **WHEN** worker instances are created
- **THEN** speed is an integer and pSkill is between 0.5 and 1.0

### Requirement: Exclusive Assignment
A worker SHALL be assigned to at most one ticket at a time and SHALL remain assigned until that ticket completes or is returned by role-specific flow.

#### Scenario: Worker cannot take second ticket
- **WHEN** a worker is already assigned to a ticket
- **THEN** the system rejects assignment of that worker to another ticket

### Requirement: Worker-Owned Ticket Transformations
Role-specific `work()` behavior SHALL own ticket transformation outcomes, including progress mutation, quality resolution, and status transition intent.

#### Scenario: Worker work call produces transformation intent
- **WHEN** the simulation invokes a worker's `work()`
- **THEN** ticket mutations and transition outcomes are determined by that worker role logic rather than by independent game-loop business rules
