# Component Descriptions - FlightDeck

FlightDeck is decomposed into five components that follow the closed 3-layer style (see `Architectural_Style_Selection.md`):
- CO1 is the presentation layer.
- CO2, CO3 and CO4 are partitions of the application layer.
- CO5 is the data layer, in front of the SQLite database.
- The Tower Status Feed is an external system.

## Components

| ID | Component | Layer | Responsibilities | Supported Functionality |
|---|---|---|---|---|
| CO1 | User Interface | Presentation | Shows the operations dashboard and each role's screens. Collects user input (logins, assignments, overrides, filters). Displays flight status, runway status, bay occupancy, stale-feed indicators, conflict alerts and the flights affected by a closure. | UC-03, UC-05, UC-06, UC-07, UC-09, UC-10, UC-13 · FR-03, FR-05, FR-06, FR-07, FR-09, FR-10, FR-15 · NFR-05, NFR-09, NFR-10, NFR-18 |
| CO2 | Access Control | Application | Authenticates users, manages sessions (15-minute expiry) and authorizes every action by role. Holds the role classes that carry out each user's actions, enforces the own-airline restriction and logs manual status overrides. | UC-16, and the entry point for UC-01, UC-04, UC-06, UC-08, UC-11, UC-14, UC-17, UC-19, UC-20, UC-21 · FR-06, FR-16 · NFR-03, NFR-07, NFR-15, NFR-16 |
| CO3 | Flight Operations | Application | Creates, edits and cancels flight records. Updates flight status manually (override) or automatically from the tower feed, and records where each status change came from. Provides flight data, including flights per airline. | UC-06, UC-07, UC-08, UC-17, UC-18 · FR-07, FR-08, FR-17, FR-18 · NFR-08, NFR-19 |
| CO4 | Resource Allocation & Audit | Application | Assigns and reassigns runways and bays. Checks for overlaps and produces conflict alerts. Handles runway closing and reopening, runway status and bay occupancy, and releases a flight's assignments when it is cancelled or rescheduled. Keeps the append-only change log and answers log queries. | UC-01, UC-02, UC-03, UC-04, UC-05, UC-11, UC-12, UC-13, UC-14, UC-15, UC-19, UC-20, UC-21 · FR-01, FR-02, FR-04, FR-05, FR-11, FR-12, FR-13, FR-14, FR-19, FR-20, FR-21 · NFR-01, NFR-04, NFR-11, NFR-13 |
| CO5 | Data Access | Data | The only component that reads and writes the SQLite database (stores D1-D5). | NFR-02, NFR-12, NFR-14, NFR-20 |

## Interfaces

Each interface operation is **identical** (name, parameters, return type) to the operation of the class that realizes it in `Object_Modeling/Consolidated_Class_Diagram.drawio`. Operations called across a component boundary take IDs, not objects. Assign/reassign operations return `ConflictAlert [0..1]`: empty means the change was saved, otherwise the alert says why it wasn't ("time overlap", "runway closed"). Close/reopen and override return the new `ChangeLogEntry`, or nothing if the request was rejected.

| Interface | Provided by (realizing class) | Required by | Operations |
|---|---|---|---|
| `IDashboard` | CO1 (`Dashboard`) | the browser user | `load(): void` · `refresh(): void` · `applyFilter(filter: FlightFilter): List<Flight>` · `clearFilter(): void` · `findFlight(flightNumber: String): Flight [0..1]` |
| `IFlightFilter` | CO1 (`FlightFilter`) | the browser user | `setAirline(code: String): void` · `setStatus(status: FlightStatus): void` · `setRunway(runwayId: String): void` · `setBay(bayNumber: int): void` · `matches(flight: Flight): boolean` · `clear(): void` |
| `IAuthentication` | CO2 (`AuthenticationService`) | CO1 | `login(userId: String, password: String): String [0..1]` · `logout(sessionId: String): void` · `isSessionActive(sessionId: String): boolean` · `getRole(sessionId: String): String` · `getAirlineScope(sessionId: String): String [0..1]` |
| `IUserActions` | CO2 (`GroundControlPersonnel`, `AirportOperationsStaff`, `AirlineCoordinator`) | CO1 | `assignRunway(sessionId, flightNumber, runwayId, startTime, endTime): ConflictAlert [0..1]` · `reassignRunway(sessionId, flightNumber, newRunwayId, reason): ConflictAlert [0..1]` · `closeRunway(sessionId, runwayId, reason): ChangeLogEntry [0..1]` · `reopenRunway(sessionId, runwayId, reason): ChangeLogEntry [0..1]` · `assignBay(sessionId, flightNumber, bayNumber: int, startTime, endTime): ConflictAlert [0..1]` · `reassignBay(sessionId, flightNumber, newBayNumber: int, reason): ConflictAlert [0..1]` · `updateBayOccupancy(sessionId, bayNumber: int, flightNumber, occupied: boolean): boolean` · `createFlight(sessionId, flightNumber, airlineCode, origin, destination, departure, arrival): Flight` · `editFlight(sessionId, flightNumber, origin, destination, departure, arrival): boolean` · `cancelFlight(sessionId, flightNumber): void` · `overrideFlightStatus(sessionId, flightNumber, newStatus: FlightStatus, reason): ChangeLogEntry [0..1]` · `viewChangeLog(sessionId, from [0..1], to [0..1], userId [0..1], flightNumber [0..1], changeType: ChangeType [0..1]): List<ChangeLogEntry>` · `viewOwnFlights(sessionId): List<Flight>` |
| `IFlightQuery` | CO3 (`FlightManager`) | CO1, CO2, CO4 | `findFlight(flightNumber: String): Flight [0..1]` · `getActiveFlights(): List<Flight>` · `getFlightsByAirline(airlineCode: String): List<Flight>` · `isFeedAvailable(): boolean` |
| `IFlightManagement` | CO3 (`FlightManager`) | CO2 | `createFlight(flightNumber, airlineCode, origin, destination, departure, arrival): Flight` · `editFlight(flightNumber, origin, destination, departure, arrival): boolean` · `cancelFlight(flightNumber): void` · `updateStatus(flightNumber, newStatus: FlightStatus, timestamp: DateTime, source: StatusSource): boolean` |
| `IResourceAllocation` | CO4 (`AllocationManager`) | CO2 | `assignRunway(flightNumber, runwayId, startTime, endTime): ConflictAlert [0..1]` · `reassignRunway(flightNumber, newRunwayId, reason, userId): ConflictAlert [0..1]` · `closeRunway(runwayId, reason, userId): ChangeLogEntry [0..1]` · `reopenRunway(runwayId, reason, userId): ChangeLogEntry [0..1]` · `assignBay(flightNumber, bayNumber: int, startTime, endTime): ConflictAlert [0..1]` · `reassignBay(flightNumber, newBayNumber: int, reason, userId): ConflictAlert [0..1]` · `updateBayOccupancy(bayNumber: int, flightNumber, occupied: boolean): boolean` · `releaseAssignments(flightNumber): void` |
| `IResourceStatus` | CO4 (`AllocationManager`) | CO1 | `getRunways(): List<Runway>` · `getRunwayStatus(runwayId: String, time: DateTime): RunwayStatus` · `getBays(): List<Bay>` · `getActiveAssignments(time: DateTime): List<Assignment>` · `getAffectedFlights(runwayId: String): List<Flight>` |
| `IChangeLog` | CO4 (`ChangeLog`) | CO2 | `recordChange(userId, changeType: ChangeType, flightNumber [0..1], resourceId [0..1], reason, previousValue, newValue): ChangeLogEntry` · `search(from [0..1], to [0..1], userId [0..1], flightNumber [0..1], changeType: ChangeType [0..1]): List<ChangeLogEntry>` |
| `IDataAccess` | CO5 (`DataStore`) | CO2, CO3, CO4 | `findUser(userId): User [0..1]` · `findFlight(flightNumber): Flight [0..1]` · `getFlights(): List<Flight>` · `saveFlight(flight: Flight): void` · `getRunways(): List<Runway>` · `saveRunway(runway: Runway): void` · `getBays(): List<Bay>` · `saveBay(bay: Bay): void` · `getAssignments(): List<Assignment>` · `saveAssignment(assignment: Assignment): void` · `appendLogEntry(entry: ChangeLogEntry): void` · `getLogEntries(): List<ChangeLogEntry>` |
| `ITowerFeed` | Tower Status Feed (external) | CO3 (`TowerStatusFeed` adapter) | `fetchUpdates(since: DateTime): List<FeedStatusUpdate>` · `isAvailable(): boolean` |

All parameters are typed in the diagrams; the table shortens `String`/`DateTime` parameter types where they're obvious from the name.

## Dependencies

All dependencies run one way, with no cycles. The presentation layer never skips the application layer:

- CO1 → CO2 (`IAuthentication`, `IUserActions`), CO1 → CO3 (`IFlightQuery`, dashboard reads only), CO1 → CO4 (`IResourceStatus`, read only)
- CO2 → CO3 (`IFlightQuery`, `IFlightManagement`), CO2 → CO4 (`IResourceAllocation`, `IChangeLog`)
- CO4 → CO3 (`IFlightQuery`)
- CO2, CO3, CO4 → CO5 (`IDataAccess`)
- CO3 → Tower Status Feed (`ITowerFeed`, external)

## Interface Consolidation Notes

The individual per-component interface diagrams (CO1-CO5 in `Component_Diagrams/`) and the final component diagram use the same interface definitions as the table above. To keep the interfaces consistent and the dependencies acyclic, we made these changes:

1. **Removed dependency cycles.** CO2 authorizes before it calls CO3 or CO4, so CO3 and CO4 don't require `IAuthorization`. CO2 logs status overrides, so CO3 doesn't require `IChangeLog`.
2. **One set of operations per interface**, used in every diagram.
3. **Merged interfaces.**
   - `IAuthorization` is internal to CO2 (`AuthenticationService.authorize`).
   - `IRunwayControl` was split: closing and reopening runways moved into `IResourceAllocation`, and the runway and bay lists moved into `IResourceStatus`.
   - `IChangeLogQuery` was merged into `IChangeLog`.
   - `IDataAccess` (CO5) was added.
   - This leaves 11 interfaces.
4. **One path for writes.** All create, update and assign actions go from CO1 through CO2's `IUserActions`, which matches the sequence diagrams. CO1 doesn't call `IFlightManagement` or `IResourceAllocation`.
5. **Airline coordinators read through CO2.** CO1 gets a coordinator's flights from `IUserActions.viewOwnFlights(sessionId)`, so CO2 enforces the own-airline restriction (FR-06). CO1 shows the operations dashboard (which reads `IFlightQuery` and `IResourceStatus` directly) only after `IAuthentication.getRole(sessionId)` returns Airport Operations Staff.
6. **Control classes realize the interfaces.** `AllocationManager` (CO4) and `FlightManager` (CO3) each run their component's use cases. Other components make one interface call into them instead of calling inner classes such as `AssignmentConflictChecker` or `Runway`.
7. **`reason` in `IChangeLog.recordChange`.** FR-14 requires a reason for every logged change.
8. **CO1's provided interfaces.** `IDashboard` and `IFlightFilter` are offered to the browser user, not to another component, so nothing in the diagram requires them.
