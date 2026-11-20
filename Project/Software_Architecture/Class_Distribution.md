# Class Distribution - FlightDeck

All 30 model elements from the consolidated class diagram (25 classes and 5 enumerations, see `Object_Modeling/Class_Inventory.md`) are assigned to exactly one component.

| ID | Component Name | Responsibilities | Included Classes |
|---|---|---|---|
| CO1 | User Interface *(Presentation layer)* | Shows the operations dashboard and each role's screens. Collects user input (logins, assignments, overrides, filters). Displays flight status, runway status, bay occupancy, stale-feed indicators, conflict alerts and the flights affected by a closure. | `Dashboard`, `FlightFilter`, `AllocationForm` |
| CO2 | Access Control *(Application layer)* | Authenticates users, manages sessions (15-minute expiry) and authorizes every action by role. Holds the role classes that carry out each user's actions (assign, reassign, override, close/reopen runway, view log). Enforces that airline coordinators only see their own airline's flights. | `User`, `AirportOperationsStaff`, `GroundControlPersonnel`, `AirlineCoordinator`, `AuthenticationService`, `Session` |
| CO3 | Flight Operations *(Application layer)* | Creates, edits and cancels flight records. Updates flight status manually (override) or automatically from the tower feed, and records where each status change came from. Provides flight data, including flights per airline. | `Flight`, `Airline`, `FlightStatus`, `StatusSource`, `TowerStatusFeed`, `FeedStatusUpdate`, `FlightManager` |
| CO4 | Resource Allocation & Audit *(Application layer)* | Assigns and reassigns runways and bays. Checks for overlaps and produces conflict alerts. Handles runway closing and reopening, runway status and bay occupancy. Keeps the append-only change log and answers log queries (date range, user ID, flight number, change type). | `AllocationManager`, `Assignment`, `RunwayAssignment`, `BayAssignment`, `Runway`, `Bay`, `RunwayStatus`, `AssignmentConflictChecker`, `ConflictAlert`, `ResourceType`, `ChangeLog`, `ChangeLogEntry`, `ChangeType` |
| CO5 | Data Access *(Data layer)* | The only component that reads and writes the SQLite database: users (D1), flights (D2), runways and runway assignments (D3), bays and bay assignments (D4) and the change log (D5). | `DataStore` |

## Per-Class Assignment

| Class ID | Name | Kind | Component |
|---|---|---|---|
| C01 | `User` | Abstract class | CO2 |
| C02 | `AirportOperationsStaff` | Class | CO2 |
| C03 | `Flight` | Class | CO3 |
| C04 | `ChangeLogEntry` | Class | CO4 |
| C06 | `BayAssignment` | Class | CO4 |
| C07 | `ConflictAlert` | Class | CO4 |
| C08 | `AssignmentConflictChecker` | Class | CO4 |
| C10 | `GroundControlPersonnel` | Class | CO2 |
| C11 | `AirlineCoordinator` | Class | CO2 |
| C12 | `AuthenticationService` | Class | CO2 |
| C13 | `Airline` | Class | CO3 |
| C14 | `Runway` | Class | CO4 |
| C16 | `TowerStatusFeed` | Class | CO3 |
| C17 | `FeedStatusUpdate` | Class | CO3 |
| C18 | `ChangeLog` | Class | CO4 |
| C19 | `Dashboard` | Class | CO1 |
| C20 | `FlightFilter` | Class | CO1 |
| C21 | `FlightStatus` | Enumeration | CO3 |
| C22 | `Bay` | Class | CO4 |
| C23 | `RunwayAssignment` | Class | CO4 |
| C24 | `Session` | Class | CO2 |
| C25 | `Assignment` | Abstract class | CO4 |
| C26 | `ChangeType` | Enumeration | CO4 |
| C27 | `StatusSource` | Enumeration | CO3 |
| C28 | `ResourceType` | Enumeration | CO4 |
| C29 | `AllocationManager` | Class | CO4 |
| C30 | `FlightManager` | Class | CO3 |
| C31 | `RunwayStatus` | Enumeration | CO4 |
| C32 | `AllocationForm` | Class | CO1 |
| C33 | `DataStore` | Class | CO5 |

**Totals:** CO1 = 3, CO2 = 6, CO3 = 7, CO4 = 13, CO5 = 1 (30 elements).

## Grouping Rationale

- **CO1:** `Dashboard`, `FlightFilter` and `AllocationForm` are the boundary classes. `Dashboard.applyFilter()` takes a `FlightFilter`, so they belong together.
- **CO2:** the `User` hierarchy, `AuthenticationService` and `Session` are linked by inheritance and the session association. All of them deal with who the user is and what they are allowed to do.
- **CO3:** `Flight`, `Airline` and the tower feed classes are linked by the "operates" association and the feed's update of flight status. `FlightManager` is the control class that realizes CO3's interfaces. `FlightStatus` and `StatusSource` are only used by `Flight` and the feed.
- **CO4:** `AllocationManager` runs every allocation use case and realizes CO4's allocation and status interfaces. The `Assignment` hierarchy, `Runway`, `Bay` and the conflict checker are closely associated with it. The change-log classes are here because four of the five `ChangeType` values (`RUNWAY_REASSIGNMENT`, `RUNWAY_CLOSURE`, `RUNWAY_REOPEN`, `BAY_REASSIGNMENT`) come from CO4, and `AllocationManager` writes them. Status overrides are logged by CO2 through `IChangeLog`, which keeps the dependency one-way.
- **CO5:** `DataStore` is the only class that touches the database.
- **No cycles:** `ChangeLogEntry` stores a `userId: String` and has no association to `User`, so CO4 doesn't depend on CO2.
