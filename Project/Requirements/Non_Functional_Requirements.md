# Non-Functional Requirements

| NFR ID | Category | Non-Functional Requirement | Contributor |
|---|---|---|---|
| NFR-01 | Performance | Runway assignment conflict checks shall complete within 2 seconds of a user submitting an assignment. | Baden |
| NFR-02 | Reliability | A confirmed runway assignment shall be persisted before confirmation is shown, so it is not lost due to a single server restart. | Baden |
| NFR-03 | Security | Only authenticated ground control personnel accounts shall be permitted to create or modify runway assignments. | Baden |
| NFR-04 | Scalability | The runway management module shall support at least 20 concurrent runway assignment operations without noticeable degradation in the demonstration environment. | Baden |
| NFR-05 | Usability | Runway conflict alerts shall be visually distinct (e.g., red highlighting) from normal status indicators so ground control can recognize them within 2 seconds of appearing. | Baden |
| NFR-06 | Performance | The dashboard shall display any change in flight status, runway occupancy, or bay allocation within 5 seconds of the change being saved, and shall load within 3 seconds with up to 200 active flights. | Musab |
| NFR-07 | Security | Manual status overrides shall be restricted to authenticated operations staff accounts, and override records (user ID, timestamp) shall be read-only and not editable or deletable by any user. | Musab |
| NFR-08 | Robustness | If the automatic tower status feed becomes unavailable, the system shall continue operating, display the last known status of each flight with a visible "stale data" indicator, and do so within 30 seconds of detecting the loss. | Musab |
| NFR-09 | Portability | The system shall run without loss of functionality in the latest stable versions of Google Chrome, Mozilla Firefox, and Microsoft Edge. | Musab |
| NFR-10 | Usability | An airline coordinator shall be able to find a flight's status and bay number in no more than 3 interactions (clicks or selections) after logging in. | Musab |
| NFR-11 | Maintainability | The bay and runway allocation logic shall be implemented as separate, independently testable modules, so a change to one does not require modifying the other. | Farah |
| NFR-12 | Reliability | Bay assignment data shall be persisted before confirmation is shown to the user, so it is not lost due to a single server restart. | Farah |
| NFR-13 | Scalability | The bay allocation module shall support tracking at least 50 bays simultaneously without noticeable degradation in the demonstration environment. | Farah |
| NFR-14 | Size | The system's demonstration dataset (flights, bays, runways, and logs) shall be structured to fit within a single lightweight database file (e.g., SQLite) not exceeding 50MB. | Farah |
| NFR-15 | Security | All conflict logs and override history (from FR-14) shall be accessible only to authenticated operations staff and shall not be exposed through any unauthenticated endpoint. | Farah |
| NFR-16 | Security | User passwords shall be stored only as hashes and never in plain text, and an authenticated session shall expire after 15 minutes of inactivity. | Satvik |
| NFR-17 | Availability | The system shall be available at least 99% of the time during scheduled demonstration and testing sessions, excluding planned maintenance announced in advance. | Satvik |
| NFR-18 | Usability | Flight statuses and conflict alerts shall not rely on color alone (each shall also carry a text label or icon), and all text shall meet a minimum contrast ratio of 4.5:1 (WCAG 2.1 AA). | Satvik |
| NFR-19 | Interoperability | The tower status feed shall be accessed through a single adapter interface, so that a simulated feed can be replaced by a real feed by changing only that adapter and no status-update logic. | Satvik |
| NFR-20 | Data Integrity | All timestamps in flight records, logs, and dashboards shall be stored in UTC and displayed with an explicit time zone label. | Satvik |
