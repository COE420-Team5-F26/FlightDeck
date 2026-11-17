# Class Distribution — FlightDeck

All 25 model elements from the Lab 5 consolidated class diagram (21 classes and 4 enumerations, see `Object_Modeling/Class_Inventory.md`) are assigned to exactly one primary component.

| ID | Component Name | Responsibilities | Included Classes |
|---|---|---|---|
| CO1 | User Interface *(Presentation layer)* | Shows the operations dashboard and each role's screens. Collects user input (assignments, overrides, filters). Displays flight status, runway and bay occupancy, and conflict alerts. | `Dashboard`, `FlightFilter` |
| CO2 | Access Control *(Application layer)* | Authenticates users, manages sessions (15-minute expiry) and authorizes actions by role. Holds the role classes that carry out each user's actions (assign, override, close runway, view log). | `User`, `AirportOperationsStaff`, `GroundControlPersonnel`, `AirlineCoordinator`, `AuthenticationService`, `Session` |
| CO3 | Flight Operations *(Application layer)* | Creates, edits and cancels flight records. Updates flight status manually (override) or automatically from the tower feed. Records where each status change came from. Restricts what each airline coordinator can see to their own airline. | `Flight`, `Airline`, `FlightStatus`, `StatusSource`, `TowerStatusFeed`, `FeedStatusUpdate` |
| CO4 | Resource Allocation & Audit *(Application layer)* | Assigns and reassigns runways and bays to flights. Checks for overlaps and produces conflict alerts. Handles runway closing and reopening and bay occupancy. Keeps the append-only change log and answers log queries. | `Assignment`, `RunwayAssignment`, `BayAssignment`, `Runway`, `Bay`, `AssignmentConflictChecker`, `ConflictAlert`, `ResourceType`, `ChangeLog`, `ChangeLogEntry`, `ChangeType` |

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

**Totals:** CO1 = 2, CO2 = 6, CO3 = 6, CO4 = 11 (25 elements).

## Grouping Rationale

- **CO1:** `Dashboard` and `FlightFilter` are the only boundary classes. `Dashboard.applyFilter()` takes a `FlightFilter`, so they belong together.
- **CO2:** the `User` hierarchy, `AuthenticationService` and `Session` are linked by inheritance and the session association. All of them deal with who the user is and what they are allowed to do.
- **CO3:** `Flight`, `Airline` and the tower feed classes are linked by the "operates" association and the feed's update of flight status. `FlightStatus` and `StatusSource` are only used by `Flight` and the feed.
- **CO4:** the `Assignment` hierarchy, `Runway`, `Bay` and the conflict checker are closely associated. The change-log classes are here because three of the four `ChangeType` values (`RUNWAY_CLOSURE`, `RUNWAY_REOPEN`, `BAY_REASSIGNMENT`) come from CO4 classes, and `Assignment` creates the audit entries. Status overrides are logged by CO2 through `IChangeLog`, which keeps the dependency one-way.
- **No cycles:** `ChangeLogEntry` stores a `userId: String`, not a `User` object, so CO4 doesn't depend on CO2.
