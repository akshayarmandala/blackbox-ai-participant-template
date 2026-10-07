# round-2 — Investigate

**Team:** BB-030
**Queries used:** 74 / 150

## What we concluded

The system output is controlled by multiple input parameters rather than a single feature. `route_age_days` has a strong effect in the tested configuration, while `shipper_score`, `recent_seizures`, and `discrepancy_ratio` also produced substantial output changes when varied individually. We found evidence that the effect of `route_age_days` can depend on `shipper_years`, suggesting a weak interaction between them. However, we do not claim interactions between other parameters unless the available experiments support them.

## How we got there

We started with controlled changes where only one parameter was modified while the remaining inputs were held constant.

The clearest `route_age_days` experiment changed it from 46.5 to 18 while keeping the other inputs fixed. The score changed from 0.9421 to 0.0430, showing that `route_age_days` is an important input.

We then tested other individual parameters. Changing `shipper_score` from 900 to 300 caused a large decrease in the output. Changing `recent_seizures` also caused a substantial output change. Changing `discrepancy_ratio` produced one of the largest observed changes. These experiments established strong feature effects without assuming real-world meaning from the feature names.

We also compared configurations involving `route_age_days` and `shipper_years`. The observed changes suggest that the effect of route age is not completely independent of shipper experience, giving evidence for a weak interaction between these two parameters.

## What we ruled out

We ruled out the hypothesis that `prior_shipments` always affects the output: in one controlled experiment, changing it from 10 to 20 produced no change in the score.

We also observed a controlled change in `transfers` that produced no measurable score change in that configuration. This means these features can be inactive or negligible under some conditions, although the experiments do not prove that they are globally ignored.

We did not treat simultaneous changes of multiple parameters as proof of interaction. When more than one input changed between two queries, we treated the result as confounded rather than assigning the score change to one particular feature or interaction.

## What we are still unsure about

We do not yet know the exact mathematical form of the system or whether the effects are globally monotonic across the full input ranges.

The direction and strength of `route_age_days` appear to depend on the surrounding configuration, so more controlled experiments are needed before describing its overall effect as simply increasing or decreasing.

The `route_age_days × shipper_years` interaction is currently supported as weak evidence rather than a fully established global rule. Other pairwise interactions remain uncertain because controlled 2×2 comparisons are required to distinguish genuine interactions from confounding.

We therefore avoid claiming interactions merely because two parameters changed together or because the output changed.
