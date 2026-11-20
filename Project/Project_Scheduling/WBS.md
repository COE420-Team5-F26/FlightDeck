# Work Breakdown Structure (WBS)

The WBS follows the scaled-down Spiral model in `Process_Model.md`. It has three cycles, and each one covers one major risk. Every cycle starts with planning and risk analysis (communication, estimation, risk-register review) and ends with a review: the increment is deployed, the team collects feedback, and the next cycle is planned.

Owners are assigned from each member's requirement areas. "All" tasks are shared, and the whole team owns the risk register (`Risk_Register.md`).

| ID | Cycle | Task Name | Description | Predecessor(s) | Duration (Days) | Owner | Work Product |
|----|-------|-----------|-------------|----------------|-----------------|------------------|--------------|
| T1 | 1 | Cycle 1 planning & risk analysis | Communication and planning for cycle 1: confirm the FRs, use cases and Level-0 DFD with stakeholders, review the risk register (focus R4, R5), and estimate and schedule the cycle. | None | 4 | All | Cycle 1 plan, reviewed risk register |
| T2 | 1 | Database design | Design the schema for the five data stores D1-D5 behind the CO5 Data Access component (`DataStore`). | T1 | 5 | Baden | Database schema |
| T3 | 1 | User-interface design | Design the role-based screens (dashboard, `AllocationForm`, flight list, override and change-log screens) for the three user roles. | T1 | 6 | Musab | Screen designs |
| T4 | 1 | Authentication and roles | Implement login, sessions with 15-minute expiry and role-based authorization (process 1.0, CO2). | T2 | 4 | Sai Satvik Veluguri | CO2 code + unit tests |
| T5 | 1 | Flight records module | Implement create, edit and cancel of flight records (process 2.0, `FlightManager` in CO3). | T4 | 6 | Sai Satvik Veluguri | CO3 flight records code + unit tests |
| T6 | 1 | Flight status and tower feed | Implement the polling tower-feed adapter, automatic status updates, the stale-data indicator and manual overrides with a reason (process 3.0; risk R4). | T5 | 5 | Sai Satvik Veluguri | CO3 status/feed code + unit tests |
| T7 | 1 | Cycle 1 review & cycle 2 planning | Deploy the cycle 1 increment and collect feedback; review the risk register (focus R5); plan cycle 2. | T3, T6 | 3 | All | Cycle 1 increment, cycle 2 plan |
| T8 | 2 | Runway assignment module | Implement runway assignment, reassignment, closure/reopening with reasons, conflict detection and concurrency control (process 4.0; risk R5). | T7 | 7 | Baden | CO4 runway code + unit tests |
| T9 | 2 | Bay and occupancy module | Implement bay assignment, bay reassignment, occupancy updates and bay conflict detection (process 5.0). | T7 | 6 | Farah | CO4 bay code + unit tests |
| T10 | 2 | Test plan | Write the test plan: test cases from use cases UC-01 to UC-21 and scenarios S-01 to S-06, including conflict and concurrency cases. | T7 | 3 | Musab | Test plan |
| T11 | 2 | Change log module | Record overrides, runway reassignments, closures/reopenings and bay reassignments, and build the change-log query screen (process 7.0). | T8, T9 | 3 | Farah | CO4 change-log code + unit tests |
| T12 | 2 | Cycle 2 review & cycle 3 planning | Deploy the cycle 2 increment and collect feedback; review the risk register; plan cycle 3. | T10, T11 | 3 | All | Cycle 2 increment, cycle 3 plan |
| T13 | 3 | Dashboard and flight views | Build the operations dashboard and the airline coordinator's own-airline flight list (process 6.0). | T12 | 6 | Musab | CO1 dashboard code |
| T14 | 3 | System integration | Connect all components through their interfaces and check the data flows against the Level-0 DFD. | T13 | 4 | All | Integrated system |
| T15 | 3 | System testing and fixes | Run the test plan for every role, including conflict, closure and override scenarios, and fix defects. | T14 | 5 | All | Test results |
| T16 | 3 | Deployment, user guide and final report | Prepare the deployment build, the user guide and the final report, and update the GitHub repository. | T15 | 4 | All | Deployed system, user guide, final report |

**Cycles:** cycle 1 = T1-T7 (days 1-27), cycle 2 = T8-T12 (days 28-40), cycle 3 = T13-T16 (days 41-59). The network schedule is in `Network_Schedule.drawio` and the Gantt chart is in `Gantt_Chart.drawio`.
