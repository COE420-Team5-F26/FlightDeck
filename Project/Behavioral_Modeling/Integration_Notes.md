## Class Model Updates from Sequence Diagram Modeling

1. **Persisting confirmed assignments:** Added `addRunwayAssignment(a:
   RunwayAssignment): void` and `addBayAssignment(a: BayAssignment): void` to
   `AssignmentConflictChecker`, which already holds the lists of existing
   assignments. A confirmed assignment is now stored before confirmation is
   shown (NFR-02, NFR-12). Two separate operations keep bay and runway logic
   independent (NFR-11). Used in UC-01, UC-04 and UC-11.

2. **Runway reassignment operation:** Added a `reassignRunway` operation to
   `GroundControlPersonnel`. This is the entry point for UC-04 and supports
   FR-04 and FR-14. (Its signature was revised in the consistency pass; see
   item 8.)

3. **Runway reassignment change type:** Added `RUNWAY_REASSIGNMENT` to the
   `ChangeType` enumeration. The enumeration previously had no value for a
   runway reassignment, although FR-04 and FR-14 require it to be logged.

4. **Assignment time window and release:** Added `getStartTime(): DateTime`,
   `getEndTime(): DateTime` and `release(): void` to the abstract `Assignment`
   class. UC-04 reads the old assignment's time window to check the new runway,
   then releases the old assignment. The operations sit in the parent class so
   that `BayAssignment` and `RunwayAssignment` share them, consistent with the
   earlier decision to generalize assignments.

## Activity Diagram Notes

5. **Lanes are lifelines.** Each swimlane is either the actor or a class that
   appears as a lifeline in the matching sequence diagram. The old "FlightDeck System" lane was not a class; its steps now
   sit in the lane of the object that carries them out (`AllocationManager`,
   `GroundControlPersonnel`, `AirportOperationsStaff`).

6. **What counts as a step.** A step belongs to the lane of the object that
   carries it out. Calls that a step makes to simple entity getters
   (`isClosed()`, `findFlight()`, `getStartTime()`) are messages in the
   sequence diagram, not separate activity steps.

## Consistency Pass

8. **Sequence diagrams redrawn** to match the final class diagram and the
   component interfaces:
   - A boundary lifeline (`:AllocationForm`) now sits after the actor 
   - Each role operation authorizes the session first and then checks the
     reason (one order in every diagram).
   - The role class makes **one** interface call into CO4
     (`:AllocationManager`), which runs the checks itself. Every message that
     crosses a component boundary is an interface operation 
   - Every message uses the exact operation name, parameters and return type
     in the class diagram. Assign/reassign operations return
     `ConflictAlert [0..1]` (empty = saved).
   - UC-04 finds the old assignment with
     `AssignmentConflictChecker.findRunwayAssignment(flight)`.
   - UC-19 lists the flights still assigned to a closed runway
     (`getAffectedFlights`), which is where UC-04 extends UC-19.
