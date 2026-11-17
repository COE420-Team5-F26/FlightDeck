# Component Descriptions — FlightDeck

FlightDeck is decomposed into four components that follow the closed 3-layer style (see `Architectural_Style_Selection.md`). CO1 is the presentation layer. CO2, CO3 and CO4 are partitions of the application layer. The SQLite database forms the data layer, and the Tower Status Feed is an external system.

## Components

| ID | Component | Layer | Responsibilities | Supported Functionality |
|---|---|---|---|---|
| CO1 | User Interface | Presentation | Shows the operations dashboard and each role's screens. Collects user input (logins, assignments, overrides, filters). Displays flight status, runway and bay occupancy, stale-feed indicators and conflict alerts. | UC-03, UC-05, UC-06, UC-07, UC-09, UC-10, UC-13 · FR-03, FR-05, FR-06, FR-07, FR-09, FR-10, FR-15 |
| CO2 | Access Control | Application | Authenticates users, manages sessions (15-minute expiry) and authorizes every action by role. Holds the role classes that carry out each user's actions and logs manual status overrides. | UC-16, and the entry point for UC-01, UC-04, UC-08, UC-11, UC-14, UC-17, UC-19, UC-20 · FR-16, NFR-03, NFR-07, NFR-16 |
| CO3 | Flight Operations | Application | Creates, edits and cancels flight records. Updates flight status manually (override) or automatically from the tower feed, and records where each status change came from. Provides flight data, including flights per airline. | UC-06, UC-07, UC-08, UC-17, UC-18 · FR-07, FR-08, FR-17, FR-18 · NFR-08, NFR-19 |
| CO4 | Resource Allocation & Audit | Application | Assigns and reassigns runways and bays to flights. Checks for overlaps and produces conflict alerts. Handles runway closing and reopening and bay occupancy. Keeps the append-only change log and answers log queries. | UC-01, UC-02, UC-03, UC-04, UC-05, UC-11, UC-12, UC-13, UC-14, UC-15, UC-19, UC-20 · FR-01, FR-02, FR-04, FR-05, FR-11, FR-12, FR-13, FR-14, FR-19, FR-20 |

## Interfaces

Full operation signatures are in the «interface» boxes of the component diagram (`Component_Diagram/`).

| Component | Provided Interfaces | Required Interfaces |
|---|---|---|
| CO1 User Interface | `IDashboard`, `IFlightFilter` | `IAuthentication`, `IUserActions` (CO2) · `IFlightQuery` (CO3) · `IResourceStatus` (CO4) |
| CO2 Access Control | `IAuthentication`, `IUserActions` | `IFlightManagement`, `IFlightQuery` (CO3) · `IResourceAllocation`, `IChangeLog` (CO4) |
| CO3 Flight Operations | `IFlightQuery`, `IFlightManagement` | `ITowerFeed` (external Tower Status Feed) |
| CO4 Resource Allocation & Audit | `IResourceAllocation`, `IResourceStatus`, `IChangeLog` | `IFlightQuery` (CO3) |

## Dependencies

All dependencies run one way, with no cycles (Lecture 08, slide 25):

- CO1 → CO2, CO3, CO4
- CO2 → CO3, CO4
- CO4 → CO3
- CO3 → Tower Status Feed (external)
- CO2, CO3, CO4 → FlightDeck DB (data layer)

## Interface Consolidation Notes

The four per-component interface diagrams were merged into one final component diagram. To keep the interfaces consistent and the dependencies acyclic, we made these changes:

1. **Removed dependency cycles.** CO2 now checks authorization before it calls CO3 or CO4, as in the Lab 6 sequence diagrams. CO3 and CO4 no longer require `IAuthorization`. CO2 logs status overrides, so CO3 no longer requires `IChangeLog`.
2. **Unified interface definitions.** Each interface now has a single set of operations used in every diagram (`IFlightQuery`, `IResourceAllocation`, `IResourceStatus`).
3. **Merged interfaces.**
   - `IAuthorization` is now internal to CO2.
   - `IRunwayControl` was split: closing and reopening runways moved into `IResourceAllocation`, and the runway and bay lists moved into `IResourceStatus`.
   - `IChangeLogQuery` was merged into `IChangeLog`.
   - This leaves 10 interfaces in total.
4. **One path for writes.** All create, update and assign actions go from CO1 through CO2's `IUserActions`, which matches the sequence diagrams. CO1 no longer calls `IFlightManagement` directly.
5. **Completed CO1's required interfaces.** CO1 now also requires `IAuthentication` and `IUserActions` from CO2.
6. **Added `reason` to `IChangeLog.recordChange`.** FR-14 requires a reason for every logged change.
