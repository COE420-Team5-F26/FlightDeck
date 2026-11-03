## Lab 6 – Class Model Updates from Sequence Diagram Modeling

1. **Persisting confirmed assignments:** Added `addRunwayAssignment(a:
   RunwayAssignment): void` and `addBayAssignment(a: BayAssignment): void` to
   `AssignmentConflictChecker`, which already holds the lists of existing
   assignments. A confirmed assignment is now stored before confirmation is
   shown (NFR-02, NFR-12). Two separate operations keep bay and runway logic
   independent (NFR-11). Used in UC-01, UC-04 and UC-11.

2. **Runway reassignment operation:** Added `reassignRunway(flight: Flight,
   newRunway: Runway, reason: String): ChangeLogEntry` to
   `GroundControlPersonnel`. This is the entry point for UC-04 and supports
   FR-04 and FR-14. It returns a `ChangeLogEntry`, like `closeRunway`,
   `reopenRunway` and `BayAssignment.reassignBay`.

3. **Runway reassignment change type:** Added `RUNWAY_REASSIGNMENT` to the
   `ChangeType` enumeration. The enumeration previously had no value for a
   runway reassignment, although FR-04, FR-14 and R-04 require it to be logged.

4. **Assignment time window and release:** Added `getStartTime(): DateTime`,
   `getEndTime(): DateTime` and `release(): void` to the abstract `Assignment`
   class. UC-04 reads the old assignment's time window to check the new runway,
   then releases the old assignment. The operations sit in the parent class so
   that `BayAssignment` and `RunwayAssignment` share them, consistent with the
   Lab 5 decision to generalize assignments.