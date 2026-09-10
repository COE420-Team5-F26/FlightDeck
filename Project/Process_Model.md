# Process Model 

## 1. Selected Software Process Model
**Spiral Model**

## 2. Justification
- **Operationally and safety relevant.** Incorrect runway assignments or bay conflicts translate to real operational hazards and costly disruptions. This is exactly the profile the Spiral model targets: risk driven development for systems where the cost of an undetected flaw is high.
- **Significant technical risk across multiple fronts.** FlightDeck must keep flight status, runway usage, and bay occupancy consistent in real time. Getting the concurrency and conflict handling logic wrong is a deeper technical risk than a typical CRUD app, and that risk needs to be identified and addressed early.
- **Requirements likely to evolve with domain understanding.** As we learn more about real airport coordination workflows (e.g., what ground control actually needs to see, how bay conflicts should be resolved), the requirements are likely to be refined. Spiral's cyclical communication–planning–modeling–construction–deployment loop is built for exactly this, unlike Waterfall's one shot planning.

## 3. Overheads / Drawbacks and Mitigation
Spiral is expensive, time consuming and requires specialized risk assessment expertise. Since this team is small these overheads are real and need to be scaled down deliberately rather than ignored:

| Overhead / Drawback | Description | Mitigation Strategy |
|---|---|---|
| High cost/time of full risk driven cycles | A full commercial Spiral process expects many iterations with deep risk analysis each time, which may prove to be difficult during a shorter timeline. | Scale down to 2-3 spiral cycles total, each covering one major risk (e.g., cycle 1: real time data consistency; cycle 2: runway/bay conflict logic; cycle 3: dashboard and integration), instead of many fine grained loops. |
| Requires risk assessment expertise | Spiral assumes access to experienced risk analysts, which we don't have. | Keep risk analysis lightweight and practical: maintain a simple risk register reviewed at the start of each cycle instead of formal risk modeling techniques. |
| Not well suited to small projects/teams | The full ceremony of Spiral can overwhelm a small team's capacity. | Combine the Planning and Modeling phases into a single short design session per cycle, and assign one member to own risk tracking so it doesn't become a full team burden. |
| Risk of over engineering low risk features | Applying the same risk rigor to every feature wastes effort where the real risk is low. | Front load the highest risk elements (data consistency, conflict handling) into the earliest cycles, and treat lower risk features (dashboard display) with a lighter approach within the later cycles. |
