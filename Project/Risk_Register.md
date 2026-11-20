# Project Risks

**Risk owner:** the whole team owns this register. The team reviews it together at the start of each spiral cycle (WBS T1, T7 and T12) and updates Probability, Impact and Status.

| Risk ID | Risk Description | Possible Cause | Probability | Impact | Mitigation/Response Strategy | Reviewed In | Status |
|---------|-------------------|-----------------|--------------|--------|-------------------------------|-------------|--------|
| R1 | Scope creep | Unclear project definition | Medium | Medium | Clear feature list/adding extra features only if we have time | T1, T7, T12 | Open |
| R2 | Uneven work distribution amongst team members | Unclear task ownership/differing skill levels | Medium | Medium | Assign clear roles (owner column in the WBS), check-ins, and team updates | T1, T7, T12 | Open |
| R3 | Security/access control flaws | Lack of clarity in roles | Low | High | Define user roles and restrict access. Build access controls | T1, T7, T12 | Open |
| R4 | Incorrect or outdated flight status causes staff to make wrong operational decisions | Delayed sync between system state and real-world events, no manual override/correction path | Medium | High | Add a manual status override for staff, and timestamp all updates so staff can judge freshness | T1 (cycle 1 focus) | Open |
| R5 | Simultaneous updates to the same flight/bay/runway conflict with each other | Multiple users interacting with shared resources with no concurrency control | Medium | High | Implement basic locking or conflict warnings; design the data model with this in mind early on | T1, T7 (cycle 2 focus) | Open |
| R6 | Tower status feed is unavailable or sends invalid data | Outage of the external (simulated) feed, malformed or unknown flight numbers | Medium | High | Keep the feed behind one adapter (NFR-19); keep showing the last known status with a "stale data" indicator (NFR-08); ignore updates for unknown flights; manual override (FR-08) as a fallback | T1, T7 | Open |
