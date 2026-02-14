## ADDED Requirements

### Requirement: Discrete Simulation Tick Loop
The system SHALL execute the simulation in discrete ticks at a fixed 500 ms interval.

#### Scenario: Tick cadence is fixed
- **WHEN** the game loop is running
- **THEN** simulation updates occur once per 500 ms tick

### Requirement: Ordered Tick Processing
The system SHALL process each tick in a deterministic order: invoke each assigned worker's `work()` exactly once, apply returned outcomes to shared state ordering, and perform project spawning checks.

#### Scenario: Tick update order
- **WHEN** a simulation tick executes
- **THEN** worker `work()` executions occur before spawning checks and shared state ordering is updated from worker outcomes

### Requirement: Centralized Runtime State
The system SHALL keep mutable simulation state in a single GameState object that includes columns, workers, projects, and tick counters.

#### Scenario: State is read from one source
- **WHEN** rendering or simulation logic reads game data
- **THEN** it uses the current GameState instance as the source of truth
