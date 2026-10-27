# Class Inventory — Airport Operations System

## Approved model elements

The approved inventory contains **25 model elements: 21 classes (19 concrete and 2 abstract) and 4 enumerations**. Class IDs are preserved from the submitted inventory; unused IDs (`C05`, `C09`, `C15`) have deliberately not been reassigned.

| Class ID | Name | Kind | Parent | Responsibility | Related use cases | Contributor |
|---|---|---|---|---|---|---|
| C01 | `User` | Abstract class | — | Represents a system user’s identity and authentication credentials; provides shared password verification and action authorization. | UC-16 | Baden |
| C02 | `AirportOperationsStaff` | Class | User | Airport operations role that can create flights, assign bays, manually override flight statuses, cancel flights, and view change logs. Inherits identity and authorization functionality. | UC-08 | Baden |
| C03 | `Flight` | Class | — | Stores flight number, airline, origin, destination, scheduled departure and arrival times, status, last status-update timestamp, and status source. Supports flight detail/status updates and cancellation. | UC-08 | Baden |
| C04 | `ChangeLogEntry` | Class | — | Records a change with entry ID, performing user ID, timestamp, reason, change type, affected flight/resource, previous value, and new value. | UC-08, UC-15, UC-19 | Baden |
| C06 | `BayAssignment` | Class | Assignment | Links a flight to a bay over a time window; inherits shared assignment behavior, supports overlap checks and bay reassignment, and records changes in ChangeLogEntry. | UC-11, UC-12, UC-15 | Farah |
| C07 | `ConflictAlert` | Class | — | Describes an unsuccessful bay/runway assignment caused by a conflict; carries conflict resource type, resource ID, message, and timestamp. | UC-13 | Farah |
| C08 | `AssignmentConflictChecker` | Class | — | Checks candidate bay and runway assignments for overlapping time windows on the same resource before confirmation and produces conflict alerts. | UC-12, UC-01, UC-04 | Farah |
| C10 | `GroundControlPersonnel` | Class | User | Ground-control role that can assign runways, close and reopen runways, and log closure/reopening changes. | UC-16, UC-19 | Satvik |
| C11 | `AirlineCoordinator` | Class | User | Read-only user role permitted to view flights belonging to its single associated Airline. | UC-16, UC-06 | Satvik |
| C12 | `AuthenticationService` | Class | — | Authenticates user ID and password, creates a Session, and authorizes actions against the user’s role. | UC-16 | Satvik |
| C13 | `Airline` | Class | — | Airline operating one or more flights (airline code and name); supports restricting coordinator views to their own airline. | UC-17, UC-06 | Satvik |
| C14 | `Runway` | Class | — | Represents a runway and its closure state; prevents acceptance of new runway assignments while closed. | UC-19, UC-05 | Satvik |
| C16 | `TowerStatusFeed` | Class | — | Receives external tower status updates, locates the corresponding flight, applies eligible updates, and ignores unknown flight numbers. | UC-18 | Satvik |
| C17 | `FeedStatusUpdate` | Class | — | Represents a tower-feed message containing a flight number, new status, and feed timestamp. | UC-18 | Satvik |
| C18 | `ChangeLog` | Class | — | Append-only collection of ChangeLogEntry records; supports newest-first queries filtered by date, user ID, and flight number. | UC-20, UC-15 | Satvik |
| C19 | `Dashboard` | Class | — | Read-only operations screen showing active flights, runway occupancy, and bay allocation; refreshes and filters current system data. | UC-09 | Musab |
| C20 | `FlightFilter` | Class | — | Carries optional airline, status, runway, and bay filtering criteria used by the dashboard and airline coordinator’s flight list. | UC-06, UC-10 | Musab |
| C21 | `FlightStatus` | Enumeration | — | Defines the permitted statuses: SCHEDULED, BOARDING, DEPARTED, LANDED, DELAYED, and CANCELLED. | UC-08, UC-18, UC-10 | Musab |
| C22 | `Bay` | Class | — | Physical aircraft parking bay with a bay number and occupied/free status; receives bay assignments and can be marked occupied/free. | UC-11, UC-12, UC-14 | Musab |
| C23 | `RunwayAssignment` | Class | Assignment | Links a flight to a runway over a time window, inheriting assignment data and overlap checks; rejects assignment when runway is closed or slot conflicts. | UC-01, UC-04 | Musab |
| C24 | `Session` | Class | — | Represents a signed-in user session, tracks session ID and last activity time, links to a user, and supports expiration checking. | UC-16 | Baden |
| C25 | `Assignment` | Abstract class | — | Common superclass for bay and runway assignments; captures ID and start/end times, tests time-window overlap/activity, and retrieves assigned flight. | UC-01 | Baden |
| C26 | `ChangeType` | Enumeration | — | Categorizes audit records as STATUS_OVERRIDE, RUNWAY_CLOSURE, RUNWAY_REOPEN, or BAY_REASSIGNMENT. | UC-08 | Baden |
| C27 | `StatusSource` | Enumeration | — | Distinguishes automatic FEED updates from MANUAL changes to flight status. | UC-08 | Baden |
| C28 | `ResourceType` | Enumeration | — | Distinguishes BAY from RUNWAY in resource-conflict alerts. | UC-12 | Baden |