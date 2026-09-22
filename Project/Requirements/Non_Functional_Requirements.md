# Non-Functional Requirements

| NFR ID | Category | Non-Functional Requirement | Contributor |
|---|---|---|---|
| NFR-01 | Performance | Runway assignment conflict checks shall complete within 2 seconds of a user submitting an assignment. | Baden |
| NFR-02 | Reliability | A confirmed runway assignment shall be persisted before confirmation is shown, so it is not lost due to a single server restart. | Baden |
| NFR-03 | Security | Only authenticated ground control personnel accounts shall be permitted to create or modify runway assignments. | Baden |
| NFR-04 | Scalability | The runway management module shall support at least 20 concurrent runway assignment operations without noticeable degradation in the demonstration environment. | Baden |
| NFR-05 | Usability | Runway conflict alerts shall be visually distinct (e.g., red highlighting) from normal status indicators so ground control can recognize them within 2 seconds of appearing. | Baden |