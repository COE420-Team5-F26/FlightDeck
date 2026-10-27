# Class Model Consistency and Traceability Check

## 1. Structural checks

| ID | Requirement/check | Result |
|---|---|---|---|
| STR-01 | Classes in inventory correspond to diagram elements | 25 approved model elements: 21 classes (including 2 abstract classes) and 4 enumerations. |
| STR-02 | No duplicate shared `User`, `Flight`, `FlightStatus` classes | Reused across individual members' responsibilities instead of copying domain classes. |
| STR-03 | Parent classes and generalization links coherent | `AirportOperationsStaff`, `GroundControlPersonnel`, and `AirlineCoordinator` inherit from `User`; `BayAssignment` and `RunwayAssignment` inherit from `Assignment`. The original PDF C06/C23 parent cells are blank and are corrected in `Class_Inventory.md`. |
| STR-04 | Attributes, method signatures, and visibility shown | Corrected draw.io class boxes show type annotations and operation parameters/returns, where applicable. |
| STR-05 | Association roles and multiplicities supplied for primary retained relationships | See association audit below. UML generalizations and dependencies do not require association multiplicities. |
| STR-06 | Ownership, association, and dependency distinguished | `ChangeLog` composes `ChangeLogEntry`; retained references use associations; temporary uses are shown as dependencies. |
| STR-07 | Enumerations distinguished from instantiable classes | `FlightStatus`, `ChangeType`, `StatusSource`, `ResourceType`. |
| STR-08 | Rules for invalid assignments and authorization recorded | Resource conflict, closed-runway, access rights, status provenance, and immutable audit notes are represented conceptually. |
| STR-09 | Readability/professional organization | Consolidated model is provided as an editable draw.io page with logical grouping and a dedicated review page. |
| STR-10 | External agreement with Labs 1–4 | Explicit assumption for this task. Keep any earlier approved names and use-case behavior authoritative if differences emerge. |

## 2. UML association and cardinality audit

| Source | Relationship | Target | Cardinality / modeling rationale |
|---|---|---|---|
| `Session` | active session | `User` | Per session: 1 user; per user: 0..1 active session (current diagram design). |
| `AirlineCoordinator` | works for | `Airline` | Per coordinator: 1 airline; per airline: 0..* coordinators. |
| `Airline` | operates | `Flight` | Per flight: 1 airline; per airline: 0..* flights. |
| `Assignment` | assigned to | `Flight` | Per assignment: 1 flight; per flight: 0..* assignments. |
| `BayAssignment` | reserves | `Bay` | Per bay assignment: 1 bay; per bay: 0..* historical assignments. |
| `RunwayAssignment` | on runway | `Runway` | Per runway assignment: 1 runway; per runway: 0..* historical assignments. |
| `ChangeLog` | composition (`entries`) | `ChangeLogEntry` | Per entry: 1 owning log; per log: 0..* entries. |
| `User` | performed by | `ChangeLogEntry` | Per entry: 1 performing user; per user: 0..* entries. |
| `AssignmentConflictChecker` | checks | `BayAssignment` | Per checker: 0..* assignment records; per checked assignment: one checker in the modeled relationship. |
| `AssignmentConflictChecker` | checks | `RunwayAssignment` | Per checker: 0..* assignment records; per checked assignment: one checker in the modeled relationship. |

## 3. Inheritance check

| Subclass | Parent | Reason for generalization |
|---|---|---|
| `AirportOperationsStaff` | `User` | A specialized authorized user, retaining shared identity/authentication behavior. |
| `GroundControlPersonnel` | `User` | A specialized authorized user for runway operations. |
| `AirlineCoordinator` | `User` | A specialized authenticated user constrained by airline scope. |
| `BayAssignment` | `Assignment` | Same assignment identity, timing and flight relationship; specializes bay allocation. |
| `RunwayAssignment` | `Assignment` | Same assignment identity, timing and flight relationship; specializes runway allocation. |