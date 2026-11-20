# Use Cases

| UC ID | Use Case Name | Primary Actor | Short Description | Contributor |
|---|---|---|---|---|
| UC-01 | Assign Runway | Ground Control Personnel | Ground control assigns an available runway to a departing or arriving flight for a given time window. | Baden |
| UC-02 | Check Runway Conflict | Ground Control Personnel | Before an assignment is confirmed, the system checks whether the runway is already assigned to another flight in an overlapping time window. | Baden |
| UC-03 | Display Runway Conflict Alert | Ground Control Personnel | When an overlap is found, the system shows an alert instead of overwriting the existing assignment, so controllers can choose another runway or time. | Baden |
| UC-04 | Reassign Runway | Ground Control Personnel | Ground control moves an already-assigned flight to a different runway (e.g., after a weather closure) and records the reason for the change. | Baden |
| UC-05 | View Runway Status | Ground Control Personnel | Ground control views the current status (free, reserved, occupied, closed) of every runway in real time. | Baden |
| UC-06 | View Own Airline's Flights | Airline Coordinator | A coordinator views the flight list, which the system automatically limits to flights operated by the coordinator's own airline. Coordinators have read-only access and cannot see other airlines' flights. | Farah |
| UC-07 | View Flight Status and Bay | Airline Coordinator | A coordinator views a flight's current status, assigned bay, and the time of the last status update. | Farah |
| UC-08 | Override Flight Status | Airport Operations Staff | Staff manually correct a flight's status (e.g., set "landed") when the automatic feed is out of date, and must enter a reason. The override is recorded with user ID, timestamp and reason. | Farah |
| UC-09 | View Airport Dashboard | Airport Operations Staff | Staff view all active flights, runway occupancy, and bay allocations on a single screen. | Farah |
| UC-10 | Filter Dashboard by Status | Airport Operations Staff | Staff narrow the dashboard flight list by status, such as "boarding" or "delayed", to prioritize which situations need attention. | Farah |
| UC-11 | Assign Bay | Ground Control Personnel, Airport Operations Staff | Staff assign an available parking bay to an arriving aircraft for a given time window. | Musab |
| UC-12 | Check Bay Conflict | Ground Control Personnel, Airport Operations Staff | Before a bay assignment is confirmed, the system checks for an overlapping assignment on the same bay. | Musab |
| UC-13 | Display Bay Conflict Alert | Ground Control Personnel, Airport Operations Staff | When a bay overlap is found, the system shows a conflict alert and does not confirm the assignment. | Musab |
| UC-14 | Update Bay Occupancy | Airport Operations Staff | Staff mark a bay "occupied" by the flight assigned to it when the aircraft arrives, and "free" when it departs. | Musab |
| UC-15 | Record Change Log Entry | None (included by UC-04, UC-08, UC-19 and UC-21) | The system writes the user ID, timestamp, and reason of every manual status override, runway reassignment, runway closure or reopening, and bay reassignment to the change log. | Musab |
| UC-16 | Log In | All users | A user authenticates with a user ID and password and gains access only to the functions permitted for their role. | Sai Satvik Veluguri |
| UC-17 | Manage Flight Records | Airport Operations Staff | Staff create, edit, or cancel a flight record (flight number, airline, origin, destination, scheduled times) so the flight appears in the flight lists and on the dashboard. Cancelling a flight, or changing its scheduled times, releases its runway and bay assignments. | Sai Satvik Veluguri |
| UC-18 | Sync Flight Status from Tower Feed | Tower Status Feed (external system) | The system receives status updates from the tower feed and updates the affected flights automatically, recording the source of each change as "feed". | Sai Satvik Veluguri |
| UC-19 | Close or Reopen Runway | Ground Control Personnel | Ground control marks a runway as closed or reopens it and must enter a reason for either action; while a runway is closed, new flight assignments to it are blocked. On closure the system lists the flights still assigned to the runway, and ground control can reassign each one (UC-04 extends UC-19). | Sai Satvik Veluguri |
| UC-20 | View Change Log | Airport Operations Staff | Staff review the change log (status overrides, runway reassignments, runway closures/reopenings and bay reassignments). They can filter by any combination of date range, user ID, flight number and change type. | Sai Satvik Veluguri |
| UC-21 | Reassign Bay | Ground Control Personnel, Airport Operations Staff | Staff move a flight's bay assignment to a different bay (e.g., when the original bay becomes unusable) and record the reason. The new bay is checked for overlaps (includes UC-12, may be extended by UC-13) and the change is logged (includes UC-15). | Team |
