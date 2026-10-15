# Core Processes Matrix

## P1 – 1.0 Authenticate Users and Roles

**Input Data**
- Login credentials (from Ground Control, Airline Coordinator, Airport Operations Staff)
- User account record (D1)

**Output Data**
- Authentication result
- Role and access level for the session

**Rules**
1. Username and password are checked against D1.
2. Every account has exactly one of three roles: Ground Control, Airline Coordinator or Airport Operations Staff.
3. The role decides which screens and actions are available. Airline Coordinators get read-only access to flights.
4. No other process runs without a valid session.

---

## P2 – 2.0 Manage Flight Records

**Input Data**
- Flight record details (create/edit/cancel) from Airport Operations Staff
- Existing flight record (D2)
- Runway availability result and allocation update confirmation (S14)
- Bay equivalents (S16)

**Output Data**
- Flight record confirmation
- New or updated flight record (D2)
- Schedule changes and flight cancellation details, including flight ID and affected time slots, to P4 (S13) and P5 (S15)

**Rules**
1. Only Airport Operations Staff can create, edit or cancel flights.
2. Each flight needs a unique flight number, airline, origin or destination, and scheduled time before it is saved.
3. A cancelled flight stays in D2 with a "Cancelled" status and is not deleted.
4. A cancelled flight releases its runway and bay.
5. Edits to scheduled time must re-check runway and bay conflicts.

---

## P3 – 3.0 Update Flight Status

**Input Data**
- Flight status updates (Tower Status Feed)
- Manual status override + reason (Airport Operations Staff)
- Flight record (D2)

**Output Data**
- Override confirmation
- Updated flight status (D2)
- Override log entry (user ID, timestamp, reason) to D5

**Rules**
1. Tower feed updates are applied automatically to the matching flight in D2.
2. A manual override needs a reason, and it is rejected if the reason is empty.
3. Every override is logged in D5 with user ID, timestamp and reason.
4. If a tower feed update and a manual override conflict, the most recent change determines the flight status. Manual overrides are logged according to Rule 3.
5. Status can only move to valid values (e.g. Scheduled, Boarding, Departed, Landed, Delayed, Cancelled).
6. Updates for flights that do not exist in D2 are ignored.

---

## P4 – 4.0 Manage Runway Assignments

**Input Data**
- Runway assignment request
- Runway reassignment request + reason
- Runway closure/reopen + reason (Ground Control)
- Runway records (D3)
- Schedule changes and cancellation details from P2 (S13)

**Output Data**
- Runway assignment confirmation
- Runway status
- Runway conflict alert
- Updated runway record (D3)
- Runway reassignment log entry to D5
- Runway availability result and allocation update confirmation to P2 (S14)

**Rules**
1. Only Ground Control can assign, reassign, close or reopen runways.
2. A runway cannot be assigned if it is closed or already booked for an overlapping time slot, so a conflict alert is sent instead.
3. Reassignments and closures/reopenings need a reason.
4. Closing a runway flags every flight still assigned to it so it can be reassigned.
5. Runway reassignments and closures are logged in D5 with user ID, timestamp and reason.

---

## P5 – 5.0 Manage Bays and Occupancy

**Input Data**
- Bay assignment request (Ground Control, Airport Operations Staff)
- Bay occupancy update (Airport Operations Staff)
- Bay records (D4)
- Schedule changes and cancellation details from P2 (S15)

**Output Data**
- Bay assignment confirmation
- Bay occupancy confirmation
- Bay conflict alert
- Updated bay record (D4)
- Bay assignment log entry to D5
- Bay availability result and allocation update confirmation to P2 (S16)

**Rules**
1. Ground Control and Airport Operations Staff can request bay assignments, but only Airport Operations Staff can update occupancy.
2. A bay can hold only one flight at a time, so a double booking triggers a bay conflict alert and the request is not saved.
3. Occupancy can only be marked "occupied" for a flight that is assigned to that bay.
4. Marking a bay as vacated frees it for the next flight.
5. Every assignment change is logged in D5 with user ID and timestamp.

---

## P6 – 6.0 Display Dashboard and Flight Views

**Input Data**
- Dashboard filter criteria (Airport Operations Staff)
- Airline filter criteria and flight lookup request (Airline Coordinator)
- Flight records (D2)
- Runway occupancy (D3)
- Bay allocations (D4)

**Output Data**
- Dashboard view
- Filtered flight list
- Flight status, bay and last update time

**Rules**
1. Airport Operations Staff see the full operations dashboard; Airline Coordinators see the filtered flight list and flight lookup only, with read-only access.
2. This process only reads from D2, D3 and D4 and never changes them.
3. Every flight shown includes its status, assigned runway, assigned bay and last update time.
4. Filters (e.g. airline, status, runway, bay) are applied to the data currently stored.
5. A lookup for a flight that does not exist returns a "flight not found" message instead of an empty screen.
6. The view must be refreshed from the stores each time it is opened or filtered, so it never shows outdated data.

---

## P7 – 7.0 Maintain and Query Change Log

**Input Data**
- Change log query (Airport Operations Staff)
- Change log records (D5)

**Output Data**
- Change log records

**Rules**
1. Only Airport Operations Staff can query the change log.
2. D5 holds the entries written by 3.0 (status overrides), 4.0 (runway reassignments/closures) and 5.0 (bay assignments), each with user ID, timestamp and, where required, a reason.
3. Log entries cannot be edited or deleted through this process.
4. Queries can be filtered by user, date range or type of change.
5. Results are returned with the newest entry first.
6. A query with no matching entries returns a "no records found" message.
