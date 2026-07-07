# Examples

This document provides complete configuration examples for different
printer types and heater capabilities.

------------------------------------------------------------------------

# Example 1 --- Low Power Heater

## Typical Hardware

- Maximum PTC temperature: 75°C
- Thermal fuse: 85°C
- Chamber target: 45--55°C

Recommended mode:

**Linear Control**

Recommended values:

```cfg
control_mode: 0

hold_ptc_delta: 5

hold_fan_speed: 0.60

prediction_horizon: 2

thermal_memory_weight: 0.015
```

Why?

A low-power heater benefits from maximum heating performance until the
chamber reaches the target.

------------------------------------------------------------------------

# Example 2 --- Medium Power Heater

## Typical Hardware

- Maximum PTC temperature: 90--100°C
- Chamber target: 50--60°C

Recommended mode:

**Hybrid Predictive Control**

```cfg
control_mode: 1

hold_ptc_delta: 5

prediction_horizon: 2.5

thermal_memory_weight: 0.020

prediction_rate_gain: 1.0
```

Why?

Hybrid mode preserves rapid heating while reducing overshoot near the
target.

------------------------------------------------------------------------

# Example 3 --- High Power Heater

## Typical Hardware

- Maximum PTC temperature: 120--130°C
- Chamber target: 50--70°C

Recommended mode:

**Full Adaptive Predictive Control**

```cfg
control_mode: 2

prediction_horizon: 3.5

thermal_memory_weight: 0.028

prediction_rate_gain: 1.2

adaptive_curve_mode: 2
```

Why?

The heater stores a significant amount of thermal energy. Predictive
unloading greatly improves stability.

------------------------------------------------------------------------

# Example 4 --- Automatic Mode

```cfg
control_mode: -1

auto_linear_headroom: 30

auto_hybrid_headroom: 55
```

Recommended for users who want the controller to automatically choose
the most appropriate algorithm.

------------------------------------------------------------------------

# Example Profiles

## Voron 2.4

Typical chamber target:

50--60°C

Recommended:

- Automatic Mode
- Smoothstep nonlinear curve
- Hold Mode enabled

------------------------------------------------------------------------

## RatRig V-Core

Typical chamber target:

50--70°C

Recommended:

- Hybrid Predictive
- Full Adaptive Predictive for powerful heaters

------------------------------------------------------------------------

## QIDI

Typical chamber target:

45--60°C

Recommended:

- Linear Control for stock heaters
- Hybrid Predictive after heater upgrades

------------------------------------------------------------------------

# Material Recommendations

  Material   Chamber Target   Suggested Mode

  ---------- ---------------- ----------------------------

  PLA        Ambient          Heater disabled
  PETG       35--45°C         Linear
  ABS        50--60°C           Hybrid
  ASA        50--60°C           Hybrid
  PA          60--70°C            Full Adaptive Predictive (powerful heater)
  PC          60--70°C            Full Adaptive Predictive

------------------------------------------------------------------------

# Validation Checklist

Before printing:

- Verify safety limits.
- Confirm chamber sensor accuracy.
- Check fan directions.
- Test emergency shutdown.
- Observe one complete heating cycle.
- Verify Hold Mode stability.

------------------------------------------------------------------------

# Benchmark Procedure

Record:

- Time to target
- Maximum overshoot
- Stabilization time
- Average hold temperature
- Heater duty cycle
- Fan duty cycle

Compare results before changing additional parameters.

------------------------------------------------------------------------

# Best Practice

Start with conservative settings.

Only modify one parameter at a time and keep a log of every successful
configuration.

Well-documented tuning is easier to reproduce and share with other
users.
