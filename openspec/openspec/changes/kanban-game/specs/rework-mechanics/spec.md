## ADDED Requirements

### Requirement: Binomial Skill Sampling
Role outcomes SHALL use binomial sampling over integer units with pSkill as the success probability for each unit.

#### Scenario: Binomial sampling applied to work units
- **WHEN** a role action resolves n integer units
- **THEN** the system computes successful units from n Bernoulli trials using pSkill

### Requirement: Oversized Ticket Discovery and Return
A Developer assigned to a ticket representing multiple underlying components SHALL return that ticket to Backlog for re-analysis instead of continuing implementation.

#### Scenario: Oversized ticket is returned
- **WHEN** a Developer starts work on a ticket with more than one underlying component
- **THEN** the ticket moves from In Progress back to Backlog

### Requirement: Testing Detection Rework Loop
When a Tester resolves a ticket with defect points, detected defects SHALL create rework in Todo equal to detected defect points, while undetected defects remain on the original ticket.

#### Scenario: Tester creates partial rework
- **WHEN** testing detects only part of a ticket's defects
- **THEN** detected defects create a Todo rework ticket and remaining defects stay on the original ticket

### Requirement: Customer Anger Rework Loop
If a ticket reaches Done with remaining defect points greater than zero, the system SHALL create a Todo rework ticket whose points equal the remaining defect points.

#### Scenario: Done ticket with defects creates customer rework
- **WHEN** a ticket is moved to Done and has remaining defects
- **THEN** a new Todo rework ticket is created with matching points

### Requirement: Hidden Internal Quality Signals
The UI SHALL NOT explicitly reveal oversized-ticket status or defect-point values to the player before the corresponding role-specific discovery occurs.

#### Scenario: Hidden defect and oversized state
- **WHEN** the player views normal board ticket cards
- **THEN** hidden internal defect and oversized signals are not shown directly
