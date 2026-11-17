# Architectural Style Selection — FlightDeck

## Selected Architectural Style

**3-Layer Architectural Style (closed)**, a special case of the Layered style. At runtime it's deployed as a web client/server system: the browser is the client, and the application server and database are on the server side.

| Layer | Role in FlightDeck |
|---|---|
| **Presentation layer** | Screens for each role: dashboard, flight list and filters, runway, bay and override forms, change-log viewer, login |
| **Application layer** | Use-case logic, split into independent partitions: access control (CO2), flight operations including the tower feed adapter (CO3), and resource allocation & audit (CO4) |
| **Data layer** | The only part that reads and writes the database (NFR-14) |

## Main Characteristics

- Each layer uses only the layer directly below it (closed). The UI never accesses the database directly.
- A layer can be split into independent partitions. In FlightDeck, access control, flight logic and resource/audit logic are kept separate.
- Components communicate only through provided and required interfaces.

## Why It Is Appropriate

- FlightDeck is a browser-based system (Scope, NFR-09), which is the typical 3-layer case: browser, web server and database.
- All data lives in one SQLite database (NFR-14), and a single data layer is the only part that touches it.
- Safety-critical rules (overlap checks, closed runways, change logging) are enforced once, on the server, where the UI can't bypass them.

## How It Supports the Design Goals

- **DG-01:** Runway and bay logic are separate classes (`RunwayAssignment`, `BayAssignment`) inside CO4 that can be tested independently (NFR-11). The tower feed sits behind one adapter interface (NFR-19).
- **DG-02:** Every assignment is saved through the data layer before confirmation is sent back (NFR-02, NFR-12). A feed failure is isolated to the feed adapter (NFR-08).

## Advantages and Limitations

**Advantages:** low coupling and high coherence, components that can be replaced (UI, database, feed), parallel team development, and central security.

**Limitations:** every request passes through each layer, which adds a small delay. The single server is a single point of failure (NFR-17). The dashboard has to poll for updates, because lower layers can't notify the UI (NFR-06).
