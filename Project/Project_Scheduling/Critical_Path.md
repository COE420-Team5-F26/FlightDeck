# Critical Path

## Critical Path

**T1 → T2 → T4 → T5 → T6 → T7 → T8 → T11 → T12 → T13 → T14 → T15 → T16**

## Expected Project Duration

**59 days** (three spiral cycles: days 1-27, 28-40 and 41-59)

## Explanation

The forward and backward passes use inclusive day numbers (EF = ES + duration − 1, LS = LF − duration + 1). The critical path is the chain of tasks whose slack (LS − ES) is 0. These tasks have identical earliest and latest dates, so a delay in any of them delays the whole project by the same amount.

**Duration:** 4 (T1) + 5 (T2) + 4 (T4) + 6 (T5) + 5 (T6) + 3 (T7) + 7 (T8) + 3 (T11) + 3 (T12) + 6 (T13) + 4 (T14) + 5 (T15) + 4 (T16) = **59 days**. This matches EF of T16 = 59 in the forward pass and LF of T16 = 59 in the backward pass.

**Why this path:**
- Cycle 1 builds the data foundation in sequence: database → authentication → flight records → status/feed. UI design (T3, EF = 10) runs in parallel and finishes long before T6 (EF = 24), so T6 drives the cycle 1 review (T7).
- In cycle 2, runway assignment (T8, 7 days, EF = 34) takes longer than the bay module (T9, EF = 33) and the test plan (T10, EF = 30). So T8 drives the change log (T11) and the cycle 2 review (T12).
- Cycle 3 is a single chain.

## Non-Critical Tasks

| Task | ES | EF | LS | LF | Slack (days) |
|------|----|----|----|----|--------------|
| T3 | 5 | 10 | 19 | 24 | 14 |
| T9 | 28 | 33 | 29 | 34 | 1 |
| T10 | 28 | 30 | 35 | 37 | 7 |

These tasks can slip by up to that many days without extending the 59-day project. Any delay beyond that, or any delay to a critical task, would extend the project. 
