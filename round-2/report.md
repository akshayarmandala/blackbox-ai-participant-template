# round-2 — Investigate

**Team:** BB-030
**Queries used:** 18  / 170

## What we concluded

The system does not appear to treat all input parameters independently.

Our experiments found that `route_age_days` and `shipper_years` show a weak interaction: the effect of `route_age_days` changes depending on `shipper_years`.

We also observed strong individual effects from several parameters, particularly `discrepancy_ratio`, `declared_value`, `recent_seizures`, and `shipper_score`.

Some parameters showed little or no observable effect in the tested configurations, including `container_count`, `prior_shipments`, and `transfers`.

The behavior is not fully explained by simple monotonic relationships. In particular, the effect of `route_age_days` changes across configurations.

## How we got there

We first varied individual parameters while keeping the remaining inputs fixed. This allowed us to separate individual feature effects from changes caused by multiple parameters moving simultaneously.

We then compared `route_age_days` at different values of `shipper_years`. The observed change in the effect of route age across shipper-experience levels provided evidence for a weak interaction between these two parameters.

We tested `declared_value` at different `discrepancy_ratio` values. The two features showed opposing effects in the tested configurations, but the available experiments were not sufficient to confidently classify this as a confirmed interaction.

We also varied `container_count`, `prior_shipments`, and `transfers` while holding the other parameters fixed. These experiments produced little or no observable change in the tested configurations, so we did not treat them as major contributors based on the available evidence.

## What we ruled out

We ruled out the hypothesis that every parameter has a large independent effect.

`container_count` produced no meaningful change in some controlled experiments, so we did not treat it as a strong feature.

`prior_shipments` likewise produced no observable change in the tested configuration.

`transfers` also produced no meaningful change in the tested configuration.

We also ruled out a simple assumption that `route_age_days` has one fixed effect across all situations. Its observed effect changes with the surrounding parameter values.

We did not claim strong interactions merely because two parameters both changed the output. Where the experiments were not sufficiently controlled, we treated the relationship as uncertain rather than overclaiming.

## What we are still unsure about

We do not yet know the complete functional form of the system.

The available queries are not sufficient to confidently determine all pairwise interactions, especially for parameters where we do not have controlled comparisons at multiple levels.

In particular, the relationships involving `declared_value × discrepancy_ratio`, `route_age_days × shipper_score`, and `recent_seizures × shipper_score` require additional controlled experiments before they can be classified as confirmed interactions.

We also do not know whether the observed `route_age_days` behavior is genuinely nonlinear or is primarily caused by interactions with other parameters.

Further queries should therefore focus on controlled experiments that vary one parameter across multiple levels while repeating the experiment at different levels of a second parameter.
