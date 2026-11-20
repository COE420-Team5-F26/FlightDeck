# Class Model Consistency and Traceability Check

## 1. Structural checks

| ID | Requirement/check | Result |
|---|---|---|
| STR-01 | Classes in inventory correspond to diagram elements | 30 model elements: 25 classes (including 2 abstract classes) and 5 enumerations. Every row of `Class_Inventory.md` is a box in `Consolidated_Class_Diagram.drawio`, and the reverse. |
| STR-02 | No duplicate shared `User`, `Flight`, `FlightStatus` classes | Reused across individual members' responsibilities instead of copying domain classes. |
| STR-03 | Parent classes and generalization links coherent | `AirportOperationsStaff`, `GroundControlPersonnel` and `AirlineCoordinator` inherit from `User`; `BayAssignment` and `RunwayAssignment` inherit from `Assignment`. The submitted inventory left the C06/C23 parent cells blank; they are filled in `Class_Inventory.md`. |
| STR-04 | Attributes, method signatures, and visibility shown | Every class box shows typed attributes and operations with parameters and return types. |
| STR-05 | Association roles and multiplicities supplied for primary retained relationships | See the association audit below. Generalizations and dependencies don't need multiplicities. |
| STR-06 | Ownership, association, and dependency distinguished | `ChangeLog` composes `ChangeLogEntry`. Object fields are associations (solid lines); parameters and local objects are dependencies (dashed). |
| STR-07 | Enumerations distinguished from instantiable classes | `FlightStatus`, `ChangeType`, `StatusSource`, `ResourceType`, `RunwayStatus`. |
| STR-08 | Rules for invalid assignments and authorization recorded | Overlaps and closed runways return a `ConflictAlert`; every role operation authorizes its session first; status changes record their `StatusSource`; the change log is append-only. |
| STR-09 | Readability/professional organization | The diagram is an editable draw.io file, colour-grouped by area (users and access, flights, allocation, audit, screens, data access). |
| STR-10 | External agreement with the requirements and DFD | Checked against FR-01 to FR-21, UC-01 to UC-21 and the Level-0 DFD in the consistency pass. Every use case is traced to at least one class in `Class_Inventory.md`. |

## 2. UML association and cardinality audit

| Source | Relationship | Target | Cardinality / modeling rationale |
|---|---|---|---|
| `Session` | belongs to | `User` | Per session: 1 user; per user: 0..1 active session. |
| `AuthenticationService` | manages | `Session` | Per service: 0..* sessions; per session: 1 service. |
| `AirlineCoordinator` | works for | `Airline` | Per coordinator: 1 airline; per airline: 0..* coordinators. |
| `Airline` | operates | `Flight` | Per flight: 1 airline; per airline: 0..* flights. |
| `Assignment` | assigned to | `Flight` | Per assignment: 1 flight; per flight: 0..* assignments over time. |
| `BayAssignment` | reserves | `Bay` | Per bay assignment: 1 bay; per bay: 0..* assignments over time. |
| `RunwayAssignment` | uses | `Runway` | Per runway assignment: 1 runway; per runway: 0..* assignments over time. |
| `AssignmentConflictChecker` | checks | `BayAssignment` | Per checker: 0..* bay assignments; per bay assignment: 1 checker. |
| `AssignmentConflictChecker` | checks | `RunwayAssignment` | Per checker: 0..* runway assignments; per runway assignment: 1 checker. |
| `ChangeLog` | composition (`entries`) | `ChangeLogEntry` | Per entry: 1 owning log; per log: 0..* entries. |
| `AllocationManager` | uses checker | `AssignmentConflictChecker` | 1 to 1. |
| `AllocationManager` | manages | `Runway` | Per manager: 0..* runways; per runway: 1 manager. |
| `AllocationManager` | manages | `Bay` | Per manager: 0..* bays; per bay: 1 manager. |
| `AllocationManager` | writes to | `ChangeLog` | 1 to 1. |
| `TowerStatusFeed` | updates through | `FlightManager` | 1 to 1. |
| `AllocationManager`, `FlightManager`, `ChangeLog`, `AuthenticationService` | persists through | `DataStore` | Each holds 1 `DataStore`; the store serves all four. |

`ChangeLogEntry` stores the performing user as `userId: String` and has **no association to `User`**. That keeps the audit classes independent of the user classes.

## 3. Inheritance check

| Subclass | Parent | Reason for generalization |
|---|---|---|
| `AirportOperationsStaff` | `User` | A specialized authorized user, retaining shared identity/authentication behavior. |
| `GroundControlPersonnel` | `User` | A specialized authorized user for runway operations. |
| `AirlineCoordinator` | `User` | A specialized authenticated user constrained by airline scope. |
| `BayAssignment` | `Assignment` | Same assignment identity, timing and flight relationship; specializes bay allocation. |
| `RunwayAssignment` | `Assignment` | Same assignment identity, timing and flight relationship; specializes runway allocation. |
