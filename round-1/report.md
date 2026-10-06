# round-1 — Observe

**Team:** GK-06
**Queries used:** 56 / 150

## What we concluded

Our experiments indicate that the hidden system does not respond monotonically to every input. Several features affect the score, and the strongest observed configuration was:

- `container_count = 0`
- `declared_value = 0`
- `discrepancy_ratio = 1`
- `prior_shipments = 0`
- `recent_seizures = 5`
- `route_age_days = 46`
- `shipper_score = 900`
- `shipper_years = 40`
- `transfers = 0`
- `port = C`

This configuration produced a score of **0.9438** with an **APPROVE** decision.

The clearest nonlinear behaviour was observed for `route_age_days`. A value of 18 produced **0.0430**, while 46 produced **0.9438**. Increasing it further did not continue improving the score: 46.5 produced **0.9421**, and 56 produced **0.9306**. This suggests a favourable region around the mid-to-upper 40s rather than a simple increasing relationship.

We also observed that `declared_value = 0` performed better than the tested value of 25. High values of `shipper_score` and `shipper_years` were also present in our strongest observed configuration.

## How we got there

We used controlled black-box queries, changing inputs and comparing the resulting scores and decisions. We progressively retained values that produced stronger outputs and combined them into higher-scoring configurations.

The strongest configuration we observed was:

- `container_count = 0`
- `declared_value = 0`
- `discrepancy_ratio = 1`
- `prior_shipments = 0`
- `recent_seizures = 5`
- `route_age_days = 46`
- `shipper_score = 900`
- `shipper_years = 40`
- `transfers = 0`
- `port = C`

For `route_age_days`, the observed results were:

- `18 → 0.0430`
- `46 → 0.9438`
- `46.5 → 0.9421`
- `56 → 0.9306`

This was important because it showed that the feature has a strong effect while also demonstrating that increasing the value beyond the favourable region can reduce the score.

For `declared_value`, the tested value of 25 was worse than 0, so 0 was retained in the high-scoring configuration.

## What we ruled out

We ruled out the assumption that maximizing every numeric feature necessarily maximizes the model output.

In particular, increasing `route_age_days` beyond 46 did not improve the score:

- `46 → 0.9438`
- `46.5 → 0.9421`
- `56 → 0.9306`

We also observed that `declared_value = 25` performed worse than `declared_value = 0` under the tested configuration.

The available `shipper_score` range was also checked, with 900 being the maximum value available to the model input.

These observations indicate that the hidden system cannot be reliably optimized by simply pushing every numerical feature to its maximum.

## What we are still unsure about

The exact optimum for `route_age_days` remains unknown. Our observations indicate that the strongest region is around the mid-to-upper 40s, but we have not performed a sufficiently fine sweep to identify the precise peak.

We have also not completely isolated the independent effect of every feature under one identical baseline. Some observed effects may depend on the values of other inputs.

Feature interactions also remain uncertain. In particular, we have not established whether `discrepancy_ratio`, `shipper_score`, `shipper_years`, and `route_age_days` amplify or weaken one another.

Our highest observed score was **0.9438**, so the exact conditions required to approach a score of 1.0 remain unknown.
