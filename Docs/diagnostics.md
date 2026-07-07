# Diagnostics

## Overview

Smart Chamber Heater provides diagnostic macros that expose the internal
state of the controller.

These commands are intended for configuration, tuning, troubleshooting
and development.

None of them modifies printer behaviour.

------------------------------------------------------------------------

# Available Macros

  -----------------------------------------------------------------------

  Macro                             Purpose

  --------------------------------- -------------------------------------

  CHAMBER_LIMITS                    Show all calculated thermal limits

  CHAMBER_CONTROL_STATUS            Show the active control algorithm and
                                    the next control decision

CHAMBER_PREDICTION_STATUS                Show all prediction-related values
-----------------------------------------------------------------------

------------------------------------------------------------------------

# CHAMBER_LIMITS

Displays every thermal limit automatically derived from the configured
thermal fuse and chamber limits.

Example:

    PTC Max
    PTC Soft Limit
    PTC Safety Limit
    PTC Release Thresholds
    Maximum Chamber Target
    Maximum Chamber Safety Limit

Use this command after changing:

- thermal_fuse_temp
- max_chamber_target

to verify the calculated values.

------------------------------------------------------------------------

# CHAMBER_CONTROL_STATUS

Displays the complete internal state of the controller.

Typical information includes:

- Requested control mode
- Selected control mode
- Chamber target
- Chamber temperature
- Heater temperature
- Thermal headroom
- Heating rate
- Effective heating rate
- PTC stored thermal energy
- Predicted chamber temperature
- Current control progress
- Next heater target
- Next chamber fan speed

This command is the primary diagnostic tool during tuning.

------------------------------------------------------------------------

# CHAMBER_PREDICTION_STATUS

Displays the prediction model values.

Typical information:

- Chamber heating rate
- Prediction horizon
- PTC thermal memory
- Predicted chamber temperature
- Current prediction progress

Use this command whenever prediction parameters are modified.

------------------------------------------------------------------------

# Interpreting the Results

## Thermal Headroom

Small values indicate a heater operating close to the requested chamber
temperature.

Large values indicate significant thermal reserve.

------------------------------------------------------------------------

## Chamber Rate

Positive:

Heating.

Near zero:

Stable chamber.

Negative:

Cooling.

------------------------------------------------------------------------

## Predicted Chamber

Represents the estimated future chamber temperature.

If significantly above the target, predictive control may begin
unloading before the chamber reaches the target.

------------------------------------------------------------------------

## Progress

Progress represents the current position of the unloading curve.

0.0

Maximum heating.

1.0

Hold Mode.

Intermediate values correspond to adaptive transitions.

------------------------------------------------------------------------

# Typical Workflow

1. Set the desired chamber target.
2. Allow the chamber to begin heating.
3. Execute:

```{=html}
<!-- -->
```

    CHAMBER_CONTROL_STATUS

4. Observe:
- Selected algorithm

- Prediction

- Next heater target
5. Adjust tuning parameters if required.

------------------------------------------------------------------------

# Troubleshooting

## Prediction starts too early

Reduce:

- Prediction Horizon
- Thermal Weight
- Rate Gain

------------------------------------------------------------------------

## Prediction starts too late

Increase:

- Prediction Horizon
- Thermal Weight
- Rate Gain

------------------------------------------------------------------------

## Unexpected Control Mode

Check:

- control_mode
- auto_linear_headroom
- auto_hybrid_headroom
- thermal_fuse_temp

------------------------------------------------------------------------

# Development

These macros are also useful when developing new algorithms.

They expose the controller state without requiring additional logging
and make it easier to compare Linear, Hybrid and Full Predictive Adaptive Predictive
behaviour under identical heating conditions.
