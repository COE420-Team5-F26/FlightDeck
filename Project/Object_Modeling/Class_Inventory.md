# Class Inventory - FlightDeck Airport Operations System

## Approved model elements

The approved inventory has **30 model elements: 25 classes (23 concrete and 2 abstract) and 5 enumerations**. Class IDs are kept from the submitted inventory. The unused IDs (`C05`, `C09`, `C15`) were deliberately not reassigned. C29-C33 were added in the consistency pass (see `Integration_Notes.md`, INT-10 to INT-14).

The diagram is `Consolidated_Class_Diagram.drawio`.

| Class ID | Name | Kind | Stereotype | Parent | Responsibility | Related use cases | Contributor |
|---|---|---|---|---|---|---|---|
| C01 | `User` | Abstract class | entity | None | A system user's identity and credentials (user ID, password hash); shared password check and role-based `canPerform`. | UC-16 | Baden |
| C02 | `AirportOperationsStaff` | Class | entity | User | Airport operations role. Can create, edit and cancel flights, assign and reassign bays, update bay occupancy, override flight statuses (with a reason) and view the change log. Each operation authorizes the session first. | UC-08, UC-11, UC-14, UC-17, UC-20, UC-21 | Baden |
| C03 | `Flight` | Class | entity | None | Stores flight number, airline, origin, destination, scheduled departure and arrival times, status, last status-update time and status source. Supports status updates, detail edits and cancellation. | UC-07, UC-08, UC-17, UC-18 | Baden |
| C04 | `ChangeLogEntry` | Class | entity | None | One change-log record: entry ID, performing user ID, timestamp, reason, change type, affected flight number and resource ID, previous and new value. | UC-08, UC-15, UC-19, UC-20 | Baden |
| C06 | `BayAssignment` | Class | entity | Assignment | Links a flight to a bay over a time window; inherits the shared assignment data and overlap check. | UC-11, UC-12, UC-21 | Farah |
| C07 | `ConflictAlert` | Class | entity | None | Describes a rejected bay/runway assignment: `conflictType` (`ResourceType`), resource ID, message (e.g. "time overlap", "runway closed") and time raised. | UC-03, UC-13 | Farah |
| C08 | `AssignmentConflictChecker` | Class | control | None | Holds the current bay and runway assignments, checks a candidate assignment for overlapping time windows on the same resource, creates conflict alerts, and finds a flight's or a runway's assignments. | UC-02, UC-12 | Farah |
| C10 | `GroundControlPersonnel` | Class | entity | User | Ground-control role. Can assign and reassign runways, close and reopen runways (each with a reason), and assign and reassign bays. Each operation authorizes the session first. | UC-01, UC-04, UC-11, UC-19, UC-21 | Sai Satvik Veluguri |
| C11 | `AirlineCoordinator` | Class | entity | User | Read-only role that can only view flights of its single associated `Airline`. | UC-06, UC-07 | Sai Satvik Veluguri |
| C12 | `AuthenticationService` | Class | control | None | Authenticates user ID and password, creates and expires `Session`s, returns role and airline scope, and authorizes each action against the user's role. | UC-16 | Sai Satvik Veluguri |
| C13 | `Airline` | Class | entity | None | Airline operating one or more flights (airline code and name); its code limits a coordinator's flight list. | UC-06, UC-17 | Sai Satvik Veluguri |
| C14 | `Runway` | Class | entity | None | A runway and its closure state and reason; closing and reopening both need a reason; a closed runway accepts no new assignments. | UC-05, UC-19 | Sai Satvik Veluguri |
| C16 | `TowerStatusFeed` | Class | control | None | Adapter for the external tower status feed (NFR-19). Polls `ITowerFeed` for new updates, applies each one through `FlightManager` with source FEED, ignores unknown flights and reports whether the feed is available (NFR-08). | UC-18 | Sai Satvik Veluguri |
| C17 | `FeedStatusUpdate` | Class | entity | None | One tower-feed message: flight number, new status and feed timestamp. | UC-18 | Sai Satvik Veluguri |
| C18 | `ChangeLog` | Class | entity | None | Append-only collection of `ChangeLogEntry` records. Records a change and answers newest-first queries filtered by any combination of date range, user ID, flight number and change type. | UC-15, UC-20 | Sai Satvik Veluguri |
| C19 | `Dashboard` | Class | boundary | None | Read-only operations screen showing active flights, runway status and bay allocation; refreshes, filters, and shows the stale-feed indicator. | UC-09, UC-10 | Musab |
| C20 | `FlightFilter` | Class | boundary | None | Optional airline, status, runway and bay criteria used by the dashboard, and to narrow a coordinator's own-airline list on screen. | UC-06, UC-10 | Musab |
| C21 | `FlightStatus` | Enumeration | N/A | None | Permitted statuses: SCHEDULED, BOARDING, DEPARTED, LANDED, DELAYED, CANCELLED. | UC-07, UC-08, UC-10, UC-18 | Musab |
| C22 | `Bay` | Class | entity | None | Physical parking bay with a bay number and occupied/free state; records which flight occupies it. | UC-11, UC-14, UC-21 | Musab |
| C23 | `RunwayAssignment` | Class | entity | Assignment | Links a flight to a runway over a time window; inherits the shared assignment data and overlap check. | UC-01, UC-02, UC-04 | Musab |
| C24 | `Session` | Class | entity | None | A signed-in user session: session ID and last activity time; belongs to one user; expires after 15 minutes of inactivity (NFR-16). | UC-16 | Baden |
| C25 | `Assignment` | Abstract class | entity | None | Common superclass for bay and runway assignments: ID, start and end time, assigned flight, released flag; overlap and activity checks; release. | UC-01, UC-04, UC-11, UC-17, UC-21 | Baden |
| C26 | `ChangeType` | Enumeration | N/A | None | Change-log categories: STATUS_OVERRIDE, RUNWAY_REASSIGNMENT, RUNWAY_CLOSURE, RUNWAY_REOPEN, BAY_REASSIGNMENT. | UC-04, UC-08, UC-15, UC-19, UC-20, UC-21 | Baden |
| C27 | `StatusSource` | Enumeration | N/A | None | Distinguishes automatic FEED updates from MANUAL status changes. | UC-08, UC-18 | Baden |
| C28 | `ResourceType` | Enumeration | N/A | None | Distinguishes BAY from RUNWAY in conflict alerts. | UC-03, UC-13 | Baden |
| C29 | `AllocationManager` | Class | control | None | Runs every runway and bay use case inside CO4: assign, reassign, close/reopen, occupancy, release, affected flights and runway status. Uses `AssignmentConflictChecker`, writes reassignments and closures to `ChangeLog`, and persists through `DataStore` before returning (NFR-02, NFR-12). | UC-01, UC-04, UC-05, UC-11, UC-14, UC-17, UC-19, UC-21 | Team |
| C30 | `FlightManager` | Class | control | None | Runs flight-record and flight-status logic inside CO3: find, list, create, edit, cancel, status update, feed availability. | UC-06, UC-07, UC-09, UC-17, UC-18 | Team |
| C31 | `RunwayStatus` | Enumeration | N/A | None | Runway status shown to users (FR-05): FREE, RESERVED, OCCUPIED, CLOSED. | UC-05, UC-09 | Team |
| C32 | `AllocationForm` | Class | boundary | None | Screen where ground control and operations staff submit runway and bay requests and see the confirmation, the conflict alert or the flights affected by a closure. | UC-01, UC-03, UC-04, UC-11, UC-13, UC-19, UC-21 | Team |
| C33 | `DataStore` | Class | data access | None | The only class that reads and writes the SQLite database (NFR-14); stores users (D1), flights (D2), runways and runway assignments (D3), bays and bay assignments (D4) and the change log (D5). | UC-01, UC-04, UC-11, UC-16, UC-17, UC-19, UC-21 | Team |
