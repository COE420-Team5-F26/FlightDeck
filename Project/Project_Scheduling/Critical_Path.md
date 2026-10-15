# Critical Path

## Critical Path

**T1 → T2 → T4 → T5 → T7 → T9 → T11 → T12 → T13**

## Expected Project Duration

**44 days**

## Explanation

From the forward and backward pass in Exercise 6, the Critical Path is the chain of activities whose Slack (LS − ES) equals 0. These are T1, T2, T4, T5, T7, T9, T11, T12 and T13. Their earliest and latest dates are identical, so any delay in one of them delays the whole project by the same amount.

**Duration:** 4 (T1) + 5 (T2) + 4 (T4) + 6 (T5) + 7 (T7) + 6 (T9) + 4 (T11) + 5 (T12) + 3 (T13) = **44 days**. This matches EF of T13 = 44 in the forward pass and LF of T13 = 44 in the backward pass.

**Why this path:** after T5, the three parallel modules finish at T6 = day 24, T8 = day 25 and T7 = day 26. T7 (runway assignment, 7 days) is the longest, so T9 and T10 must wait for it, which makes T7 the driver of the schedule. T9 (EF = 32) finishes later than T10 (EF = 29), so T9 drives T11 and the final tasks T12 and T13.

## Non-Critical Tasks

| Task | Slack (days) |
|------|--------------|
| T3 | 16 |
| T6 | 2 |
| T8 | 1 |
| T10 | 3 |

These tasks can slip by up to that many days without extending the 44-day project. Any delay beyond that, or any delay to a critical task, would extend the project.
