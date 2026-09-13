# Project Risks

| Risk ID | Risk Description | Possible Cause | Probability | Impact | Mitigation/Response Strategy |
|---------|-------------------|-----------------|--------------|--------|-------------------------------|
| R1 | Scope creep | Unclear project definition | Medium | Medium | Clear feature list/adding extra features only if we have time |
| R2 | Uneven work distribution amongst team members | Unclear task ownership/differing skill levels | Medium | Medium | Assign clear roles, check-ins, and team updates |
| R3 | Security/access control flaws | Lack of clarity in roles | Low | High | Define user roles and restrict access. Build access controls |
| R4 | Incorrect or outdated flight status causes staff to make wrong operational decisions | Delayed sync between system state and real-world events, no manual override/correction path | Medium | High | Add a manual status override for staff, and timestamp all updates so staff can judge freshness |
| R5 | Simultaneous updates to the same flight/bay/runway conflict with each other | Multiple users interacting with shared resources with no concurrency control | Medium | High | Implement basic locking or conflict warnings; design the data model with this in mind early on |