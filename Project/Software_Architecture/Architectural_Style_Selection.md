# Architectural Style Selection - FlightDeck

## Selected Architectural Style

**3-Layer Architectural Style (closed)**, a special case of the Layered style. At runtime it's deployed as a web client/server system: the browser is the client, and the application server and database are on the server side.

| Layer | Role in FlightDeck |
|---|---|
| **Presentation layer** | CO1 User Interface: screens for each role (dashboard, flight list and filters, `AllocationForm` for runway and bay requests, override form, change-log viewer, login) |
| **Application layer** | Use-case logic, split into independent partitions: access control (CO2), flight operations including the tower feed adapter (CO3), and resource allocation & audit (CO4) |
| **Data layer** | CO5 Data Access (`DataStore`): the only component that reads and writes the SQLite database (NFR-14) |

## Main Characteristics

- Each layer uses only the layer directly below it (closed). The UI never accesses CO5 or the database.
- A layer can be split into independent partitions. In FlightDeck, access control, flight logic and resource/audit logic are kept separate, and partitions call each other only through interfaces, in one direction (CO2 → CO3, CO2 → CO4, CO4 → CO3).
- Components communicate only through provided and required interfaces.

## Why It Is Appropriate

- FlightDeck is a browser-based system (Scope, NFR-09), which is the typical 3-layer case: browser, web server and database.
- All data lives in one SQLite database (NFR-14), and a single data-access component is the only part that touches it.
- Safety-critical rules (overlap checks, closed runways, change logging, authorization) are enforced once, on the server, where the UI can't bypass them.

## How It Supports the Design Goals

- **DG-01:** Runway and bay logic are separate classes (`RunwayAssignment`, `BayAssignment`) inside CO4 that can be tested independently (NFR-11). The tower feed sits behind one adapter interface (NFR-19).
- **DG-02:** Every assignment is saved through CO5's `IDataAccess` before confirmation is sent back (NFR-02, NFR-12). A feed failure is isolated to the feed adapter (NFR-08).
- **DG-03:** CO2 authorizes every write and every coordinator read in one place.

## Alternatives Considered

| Style | Why not chosen |
|---|---|
| Client/Server only | It fits the deployment (and we do deploy this way), but on its own it says nothing about how the server is organised. |
| Repository | All components would share one data model directly. That gives low coupling between components, but every component would depend on the schema, which works against DG-01. |
| MVC | It would let the model push updates to the dashboard instead of polling, but it adds complexity, and NFR-06 (5 s) is met by polling. |

## Advantages and Limitations

**Advantages:** low coupling and high coherence, components that can be replaced (UI, database, feed), parallel team development, and central security.

**Limitations:** every request passes through each layer, which adds a small delay. The single server is a single point of failure (NFR-17). The dashboard has to poll for updates, because lower layers can't notify the UI (NFR-06).
