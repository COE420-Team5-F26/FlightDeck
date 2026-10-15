# DFD Consistency Check

## Context Diagram Flows (Exercise 1)

| Code | Entity / process | Data flow contents |
|------|------------------|--------------------|
| C1 | Airline Coordinator → 0 | Login credentials; airline filter criteria; flight lookup request |
| C2 | 0 → Airline Coordinator | Authentication result; filtered flight list; flight status, bay, last update time |
| C3 | Ground Control Personnel → 0 | Login credentials; runway assignment request; runway reassignment request + reason; runway closure/reopen + reason; bay assignment request |
| C4 | 0 → Ground Control Personnel | Authentication result; runway assignment confirmation; runway status; runway conflict alert; bay assignment confirmation; bay conflict alert |
| C5 | Airport Operations Staff → 0 | Login credentials; flight record details (create/edit/cancel); manual status override + reason; bay assignment request; bay occupancy update; dashboard filter criteria; change log query |
| C6 | 0 → Airport Operations Staff | Authentication result; flight record confirmation; override confirmation; bay assignment confirmation; bay occupancy confirmation; bay conflict alert; dashboard view; change log records |
| C7 | Tower Status Feed → 0 | Flight status updates |

## Level-0 DFD: External Entity Flows (balanced with the Context Diagram)

| Code | Entity / Process | Data flow contents |
|------|------------------|--------------------|
| E1a | Ground Control Personnel → 1.0 | Login credentials |
| E1b | 1.0 → Ground Control Personnel | Authentication result |
| E2a | Airline Coordinator → 1.0 | Login credentials |
| E2b | 1.0 → Airline Coordinator | Authentication result |
| E3a | Airport Operations Staff → 1.0 | Login credentials |
| E3b | 1.0 → Airport Operations Staff | Authentication result |
| E4a | Ground Control Personnel → 4.0 | Runway assignment request; runway reassignment request + reason; runway closure/reopen request + reason |
| E4b | 4.0 → Ground Control Personnel | Runway assignment confirmation; runway status; runway conflict alert |
| E5a | Ground Control Personnel → 5.0 | Bay assignment request |
| E5b | 5.0 → Ground Control Personnel | Bay assignment confirmation; bay conflict alert |
| E6a | Airport Operations Staff → 5.0 | Bay assignment request; bay occupancy update |
| E6b | 5.0 → Airport Operations Staff | Bay assignment confirmation; bay occupancy confirmation; bay conflict alert |
| E7a | Airport Operations Staff → 2.0 | Flight record details (create/edit/cancel) |
| E7b | 2.0 → Airport Operations Staff | Flight record confirmation |
| E8a | Airport Operations Staff → 3.0 | Manual status override + reason |
| E8b | 3.0 → Airport Operations Staff | Override confirmation |
| E9a | Airport Operations Staff → 6.0 | Dashboard filter criteria |
| E9b | 6.0 → Airport Operations Staff | Dashboard view |
| E10a | Airline Coordinator → 6.0 | Airline filter criteria; flight lookup request |
| E10b | 6.0 → Airline Coordinator | Filtered flight list; flight status, bay and last update time |
| E11a | Airport Operations Staff → 7.0 | Change log query |
| E11b | 7.0 → Airport Operations Staff | Change log records |
| E12 | Tower Status Feed → 3.0 | Flight status updates |

## Level-0 DFD: Data Store and Internal Flows

| Code | Entity / Process | Data flow contents |
|------|------------------|--------------------|
| S1a | 1.0 → D1 | Credential check |
| S1b | D1 → 1.0 | User account record |
| S2a | 2.0 → D2 | Flight record write |
| S2b | D2 → 2.0 | Flight record |
| S3a | 3.0 → D2 | Status update |
| S3b | D2 → 3.0 | Flight record |
| S4a | 4.0 → D3 | Runway assignment/status update |
| S4b | D3 → 4.0 | Runway record/status |
| S5a | 5.0 → D4 | Bay assignment/occupancy update |
| S5b | D4 → 5.0 | Bay record/occupancy |
| S6 | D2 → 6.0 | Flight records |
| S7 | D3 → 6.0 | Runway occupancy |
| S8 | D4 → 6.0 | Bay allocations |
| S9 | D5 → 7.0 | Change log records |
| S10 | 3.0 → D5 | Override log entry (user ID, timestamp and reason) |
| S11 | 4.0 → D5 | Runway reassignment/closure log entry |
| S12 | 5.0 → D5 | Bay assignment log entry |
| S13 | 2.0 → 4.0 | Proposed schedule change; confirmed schedule change; flight cancellation details (flight ID and affected time slots) |
| S14 | 4.0 → 2.0 | Runway availability/conflict result; runway allocation update/release confirmation |
| S15 | 2.0 → 5.0 | Proposed schedule change; confirmed schedule change; flight cancellation details (flight ID and affected time slots) |
| S16 | 5.0 → 2.0 | Bay availability/conflict result; bay allocation update/release confirmation |
