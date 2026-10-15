# Work Breakdown Structure (WBS)

| ID | Task Name | Description | Predecessor(s) | Duration (Days) |
|----|-----------|-------------|----------------|-----------------|
| T1 | Requirements refinement | Finalise the functional requirements and use cases from Lab 3 and confirm the Level-0 DFD processes (1.0–7.0) and the three user roles. | — | 4 |
| T2 | Database design | Design the schema for the five data stores: user accounts (D1), flight records (D2), runway records (D3), bay records (D4) and change log (D5). | T1 | 5 |
| T3 | User-interface design | Design the role-based screens for Ground Control, Airline Coordinator and Airport Operations Staff, including the dashboard layout. | T1 | 6 |
| T4 | Authentication and roles | Implement login and role-based access control (process 1.0) on top of the user accounts store. | T2 | 4 |
| T5 | Flight records module | Implement create, edit and cancel of flight records (process 2.0); every other module reads or updates these records. | T4 | 6 |
| T6 | Flight status and tower feed | Implement automatic status updates from the tower status feed and manual overrides with a reason (process 3.0). | T5 | 5 |
| T7 | Runway assignment module | Implement runway assignment, reassignment, closure/reopening and runway conflict detection (process 4.0). | T5 | 7 |
| T8 | Bay and occupancy module | Implement bay assignment, occupancy updates and bay conflict detection (process 5.0). | T5 | 6 |
| T9 | Dashboard and flight views | Build the operations dashboard and the airline-filtered flight list showing status, runway and bay data (process 6.0). | T3, T6, T7, T8 | 6 |
| T10 | Change log module | Record override, runway and bay changes with user ID, timestamp and reason, and build the change log query screen (process 7.0). | T6, T7, T8 | 3 |
| T11 | System integration | Connect all modules end to end and check that the data flows between them match the Level-0 DFD. | T9, T10 | 4 |
| T12 | System testing and fixes | Test the system against the use cases for each role, including conflict and override scenarios, and fix defects. | T11 | 5 |
| T13 | Deployment and documentation | Prepare the deployment build, user guide and final GitHub repository update. | T12 | 3 |
