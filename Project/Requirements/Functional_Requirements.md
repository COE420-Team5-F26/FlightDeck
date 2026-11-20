# Functional Requirements

| FR ID | Functional Requirement | Source Scenario/Stakeholder | Contributor |
|---|---|---|---|
| FR-01 | The system shall allow ground control personnel to assign an available runway to a departing or arriving flight. | S-01 / Ground Control | Baden |
| FR-02 | The system shall prevent two flights from being assigned to the same runway for overlapping time windows. | S-03 / Ground Control | Baden |
| FR-03 | The system shall display a conflict alert to ground control personnel when a runway assignment would overlap with an existing assignment. | S-03 / Ground Control | Baden |
| FR-04 | The system shall allow ground control personnel to reassign a flight already assigned to a runway to a different runway, and record the reason for the change. | S-01 / Ground Control | Baden |
| FR-05 | The system shall display the current status (free, reserved, occupied, or closed) of every runway in real time. | S-01 / Ground Control | Baden |
| FR-06 | The system shall restrict each airline coordinator to read-only viewing of flights operated by the coordinator's own airline, and shall not show them flights of any other airline. | S-02 / Airline Coordinator | Musab |
| FR-07 | The system shall display, for each flight, its current status (e.g., scheduled, boarding, delayed, departed, landed), its assigned bay number, and the timestamp of the last status update. | S-02 / Airline Coordinator | Musab |
| FR-08 | The system shall allow authorized airport operations staff to manually override a flight's status, shall require a reason for each override, and shall record the user ID, timestamp and reason of each override. | S-04 / Airport Operations Staff | Musab |
| FR-09 | The system shall provide a centralized dashboard that displays all active flights with their statuses, runway occupancy, and bay allocations on a single screen. | S-05 / Airport Operations Staff | Musab |
| FR-10 | The system shall allow airport operations staff to filter the dashboard flight list by status (e.g., "boarding" or "delayed"). | S-05 / Airport Operations Staff | Musab |
| FR-11 | The system shall allow ground control personnel or operations staff to assign an available parking bay to an arriving aircraft. | S-01 / Ground Control | Farah |
| FR-12 | The system shall prevent two aircraft from being assigned to the same bay at overlapping times. | S-03 / Ground Control | Farah |
| FR-13 | The system shall allow authorized staff to mark a bay as "occupied" by the flight assigned to it upon aircraft arrival, and "free" upon aircraft departure. | S-01 / Airport Operations Staff | Farah |
| FR-14 | The system shall maintain a change log of all manual status overrides, runway reassignments, runway closures and reopenings, and bay reassignments, including the user ID, timestamp, and reason for each change. First-time runway and bay assignments are not logged. | S-04 / Airport Operations Staff | Farah |
| FR-15 | The system shall display a conflict alert when a bay assignment would overlap with an existing bay assignment. | S-03 / Ground Control | Farah |
| FR-16 | The system shall require every user to log in with a unique user ID and password before accessing any function, and shall restrict each account to the functions permitted for its role (ground control personnel, airline coordinator, or airport operations staff). | S-02 / All Stakeholders | Sai Satvik Veluguri |
| FR-17 | The system shall allow authorized airport operations staff to create, edit, and cancel flight records, each containing at minimum the flight number, airline, origin, destination, and scheduled departure and arrival times. Cancelling a flight shall release its runway and bay assignments, and changing its scheduled times shall release them so the flight is assigned again with a fresh conflict check. | S-05 / Airport Operations Staff | Sai Satvik Veluguri |
| FR-18 | The system shall automatically update a flight's status when a new status is received from the tower status feed, and shall record whether each status change originated from the feed or from a manual override. | S-04 / Airport Operations Staff | Sai Satvik Veluguri |
| FR-19 | The system shall allow ground control personnel to mark a runway as closed or reopen it, shall require and record a reason for each closure and each reopening, shall prevent new flight assignments to a runway while it is closed, and on closure shall list the flights still assigned to that runway so they can be reassigned (FR-04). | S-01 / Ground Control | Sai Satvik Veluguri |
| FR-20 | The system shall allow authorized airport operations staff to view the change log (see FR-14) and filter it by any combination of date range, user ID, flight number, and change type, where an empty filter matches all entries. | S-04 / Airport Operations Staff | Sai Satvik Veluguri |
| FR-21 | The system shall allow ground control personnel or airport operations staff to move a flight's bay assignment to a different available bay, shall check the new bay for overlapping assignments (see FR-12 and FR-15) before confirming, and shall record the reason for the change. | S-06 / Airport Operations Staff | Team |

