# Round 4 — Final Reconstruction

**Team:** BB-030

## Objective

The objective of Round 4 was to reconstruct the behaviour of the GK-06 black-box system using observations collected across the previous rounds and additional Round 4 queries.

## Data used

We used the query/output observations collected during the investigation rounds. Each observation contains the ten input parameters and the corresponding black-box score and decision.

The input parameters are:

- `container_count`
- `declared_value`
- `discrepancy_ratio`
- `port`
- `prior_shipments`
- `recent_seizures`
- `route_age_days`
- `shipper_score`
- `shipper_years`
- `transfers`

## Reconstruction approach

We treated the feature names only as labels and based the reconstruction on observed input/output behaviour.

The reconstruction process was:

1. Collect the observed query inputs and outputs.
2. Encode the categorical `port` feature.
3. Separate the score from the APPROVE/DECLINE decision.
4. Analyse individual feature effects.
5. Test possible feature interactions.
6. Train candidate models on the collected observations.
7. Compare their predictions against the observed black-box outputs.
8. Select the model that best reproduced the observed behaviour.
9. Test the reconstruction on observations that were not used during fitting.

## Important observations

The experiments showed that several parameters have measurable effects on the output.

`route_age_days` produced a particularly large change in controlled experiments. For example, with the other inputs held fixed, changing `route_age_days` from 46.5 to 18 changed the score from 0.9421 to 0.0430.

`shipper_score`, `recent_seizures`, and `discrepancy_ratio` also produced substantial changes in controlled experiments.

The observations also indicate that feature effects are not necessarily independent. In particular, the behaviour involving `route_age_days` and `shipper_years` suggests a weak interaction.

We did not treat a simultaneous change in multiple parameters as proof of an interaction.

## Model selection

Candidate reconstruction models were compared using prediction error on held-out observations.

The selected reconstruction was the model that provided the closest reproduction of the observed black-box outputs while avoiding unsupported assumptions about the feature relationships.

## Testing

The final reconstruction was tested against observations that were not used to fit the model.

We compared:

- predicted score vs observed score
- predicted decision vs observed decision
- absolute score error
- decision accuracy

The notebook contains the complete preprocessing, training, comparison, and testing procedure.

## Limitations

The reconstruction is an approximation of the black box rather than a guaranteed recovery of its original implementation.

Some feature interactions remain uncertain because proving an interaction requires controlled comparisons where the relevant variables are varied independently.

The model should therefore be judged by how accurately it reproduces the observed system behaviour rather than by assumptions about what the feature names mean.

## Conclusion

The investigation established that GK-06 depends on multiple input parameters and that some effects are context-dependent. The Round 4 reconstruction uses the accumulated observations to approximate this behaviour and is evaluated against held-out black-box observations.
