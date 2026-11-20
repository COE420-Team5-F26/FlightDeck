# Integration Notes - FlightDeck Airport Operations System

Combine the individual flight-status, authentication, dashboard, runway, bay-assignment, and conflict-management models into **one coherent UML class diagram**. Resolve duplicate class definitions, unify names and responsibilities, preserve useful inheritance, and distinguish persistent associations from temporary method dependencies.

## Integration decision log

| ID | Topic | Decision and rationale | Affected elements | Status |
|---|---|---|---|---|
| INT-01 | Shared user identity | Maintain one `User` superclass, with `AirportOperationsStaff`, `GroundControlPersonnel`, and `AirlineCoordinator` as specialized user roles, so credentials and identity are not duplicated. | C01, C02, C10, C11 | Applied |
| INT-02 | Shared flight model | Use one `Flight` class across status override, tower feed, airline access, allocations, and dashboard. Keep `FlightStatus` as an enumeration and add the typed `StatusSource` distinction. | C03, C21, C27 | Applied |
| INT-03 | Assignment commonality | Introduce **abstract** `Assignment` holding assignment ID, start and end times, assigned flight, and common time checks. Generalize `BayAssignment` and `RunwayAssignment` from it. | C06, C23, C25 | Applied |
| INT-04 | Consolidated audit entries | Replace distinct final-model classes for `FlightStatusOverrideEntry`, `BayReassignmentEntry`, and `RunwayClosureEntry` with `ChangeLogEntry`; distinguish cases through `ChangeType`. | C04, C18, C26 | Applied |
| INT-05 | Login/session | Add `Session` to represent sessions returned by `AuthenticationService` and associated with `User`. | C01, C12, C24 | Applied |
| INT-06 | Conflict typing | Keep one conflict checker and one alert type; identify the relevant resource with the `ResourceType` enumeration (`BAY` or `RUNWAY`). | C07, C08, C28 | Applied |
| INT-07 | Ownership and use | Treat retained object relationships as UML associations, owned audit entries as composition, and temporary calls/parameters as UML dependencies (as shown in the revised diagram). | Model-wide | Applied in diagram |
| INT-08 | Candidate-class parent fixes | Populate `Parent Class = Assignment` for `BayAssignment` and `RunwayAssignment` in the consolidated Markdown inventory. Their parent cells were blank in the submitted inventory table, although the inherited behavior was described. | C06, C23 | Corrected in Markdown |
| INT-09 | Central flight read model | Keep `Dashboard` read-only and use `FlightFilter` to supply optional filtering criteria; do not duplicate `Flight` for each view. | C03, C19, C20 | Applied |
| INT-10 | Control class for allocation (consistency pass) | Add `AllocationManager` to run every runway and bay use case inside one class, so other components make one call into it instead of calling `AssignmentConflictChecker`, `Runway` and `ChangeLog` directly. | C29 | Applied |
| INT-11 | Control class for flights (consistency pass) | Add `FlightManager` to own flight lookups, edits and status updates; `TowerStatusFeed` and `Dashboard` use it instead of each having their own `findFlight`. | C30, C16, C19 | Applied |
| INT-12 | Boundary and data-access classes (consistency pass) | Add `AllocationForm` (boundary for runway/bay requests) and `DataStore` (the only class that touches the database). | C32, C33 | Applied |
| INT-13 | Runway status (consistency pass) | Add the `RunwayStatus` enumeration (FREE, RESERVED, OCCUPIED, CLOSED) for FR-05/UC-05. | C31 | Applied |
| INT-14 | Audit independence (consistency pass) | Remove the `User`-`ChangeLogEntry` association; the entry keeps `userId: String`. | C01, C04 | Applied |

## Duplicate classes and design differences resolved

| Original model difference | Resolution in consolidated model |
|---|---|
| `User`, `Flight`, and `FlightStatus` appeared across different members' individual class diagrams. | Reuse exactly one definition of each approved element. |
| Different change-log entry subtypes appeared in individual model work. | Use `ChangeLogEntry` + `ChangeType`; preserve details such as reason, actor, timestamp, previous/new values. |
| `BayAssignment` and `RunwayAssignment` each carried overlapping assignment properties/logic. | Lift shared state and operations into abstract `Assignment`. |
| Different services read or update the same flight resource. | Preserve a common flight object and express other objects' access using associations/dependencies as appropriate. |
| Enumerated values were represented inconsistently across views. | Use one `FlightStatus`, plus `StatusSource`, `ChangeType`, and `ResourceType` as typed enumerations. |

## Core association and multiplicity decisions

The relationship ends and their multiplicities below reflect the **corrected diagram**, not a separately validated implementation:

| Relationship | Interpreted UML constraint |
|---|---|
| `Session` - `User` | Each session belongs to **1 user**; each user has **0..1 active sessions** in this model. |
| `AirlineCoordinator` - `Airline` | Each coordinator works for **1 airline**; an airline has **0..*** coordinators. |
| `Airline` - `Flight` | Each flight belongs to **1 airline**; an airline operates **0..*** flights. |
| `Assignment` - `Flight` | Each assignment refers to **1 flight**; each flight can have **0..*** assignments over time. |
| `BayAssignment` - `Bay` | Each bay assignment reserves **1 bay**; a bay can have **0..*** assignment records over time. |
| `RunwayAssignment` - `Runway` | Each runway assignment uses **1 runway**; a runway can have **0..*** assignment records over time. |
| `ChangeLog` - `ChangeLogEntry` (composition) | A log owns **0..*** entries; each entry belongs to **1 log**. |
