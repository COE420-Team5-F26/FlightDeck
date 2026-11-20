# Architectural Design Goals - FlightDeck

The design goals come from FlightDeck's non-functional requirements. We use them to compare alternative designs and to make design trade-offs.

| Goal ID | Design Goal | Description | Related Requirement | Priority | Justification |
|---|---|---|---|---|---|
| DG-01 | Maintainability / Modularity | The system is split into highly coherent components with low coupling that communicate only through narrow, well-defined interfaces. One component (e.g. the bay allocation logic, the tower feed source or the database) can change without changing the others. | NFR-11 (bay and runway allocation in separate, independently testable modules), NFR-19 (tower feed behind a single adapter interface) | High | Two requirements ask for modular boundaries directly. The team builds in spiral iterations, so components are changed and retested often. Four team members can also work on separate components in parallel. |
| DG-02 | Reliability | No confirmed runway or bay assignment is lost. Data is persisted before the user sees confirmation, and the system keeps running when the external tower feed fails. | NFR-02, NFR-12 (persist before confirming), NFR-08 (keep operating with "stale data" if the feed is lost), NFR-17 (availability) | High | Runway and bay assignment is FlightDeck's safety-critical function (UC-01, UC-04, UC-11). A lost assignment could put two aircraft on the same runway or bay. Concurrency and state synchronization are also risks R4, R5 and R6 in `Risk_Register.md`. |
| DG-03 | Security | Every action is authorized against the user's role in one place, and audit data can't be changed or read without authorization. | NFR-03, NFR-07, NFR-15, NFR-16, FR-06, FR-16 | High | Wrong access (e.g. a coordinator seeing another airline, or someone editing the change log) would break trust in the system (risk R3). |

## Design Trade-offs

| Trade-off | Choice | Reason |
|---|---|---|
| Maintainability vs runtime efficiency | **Closed** layers (maintainability) | Each request passes through every layer, which adds a little delay. NFR-01 (2 s) and NFR-06 (5 s) leave plenty of room, and DG-01 is a high priority. |
| Rapid development vs functionality | Simulated tower feed behind one adapter | A real feed is out of scope; the adapter (NFR-19) lets a real feed be added later without touching other code. |
| Cost vs robustness | One server and one SQLite file (NFR-14, NFR-17) | No paid infrastructure; robustness comes from persisting before confirming (DG-02), not from redundant servers. |
