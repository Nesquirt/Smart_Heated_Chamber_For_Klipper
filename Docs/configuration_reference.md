# Configuration Reference

## Overview

This document describes every user-configurable parameter available in
Smart Chamber Heater.

The configuration has been designed so that every printer can be tuned
without modifying the controller logic.

------------------------------------------------------------------------

# Configuration Philosophy

All user-editable parameters are grouped together inside the
`_CHAMBERVARS` macro.

Only these variables should normally be changed.

Internal variables are automatically managed by the controller.

------------------------------------------------------------------------

# Heater Configuration

## thermal_fuse_temp

Defines the temperature rating of the installed thermal fuse.

**Required**

Recommended: Use the exact value printed on the fuse.

Changing this value changes all calculated protection thresholds.

Never configure a value higher than the installed fuse.

------------------------------------------------------------------------

## max_chamber_target

Maximum chamber temperature allowed by the controller.

Recommended: Choose the highest chamber temperature considered safe for
your printer.
(10 degree less your maximal rated temperature on thermal fuse)

------------------------------------------------------------------------

## heater_min

Minimum heater target that can be commanded.

Increasing this value keeps the PTC warmer.

Lower values reduce standby temperature.

------------------------------------------------------------------------

# Hold Mode

## hold_enabled

Enables or disables Hold Mode.

Values:

- 0 = Disabled
- 1 = Enabled

Recommended: Enabled.

------------------------------------------------------------------------

## hold_ptc_delta

Difference between chamber target and hold PTC temperature.

Example:

Target = 50°C

Delta = 5°C

Hold PTC = 55°C

Increase:

- Better stability
- Less chamber drop

Decrease:

- Lower energy consumption
- Lower overshoot

------------------------------------------------------------------------

## hold_fan_speed

Fan speed used during Hold Mode.

Range:

0.0 -- 1.0

Higher values improve temperature uniformity.

Lower values reduce airflow.

------------------------------------------------------------------------

# Control Mode

## control_mode

Selects the active control algorithm.

Values:

- -1 = Automatic
- 0 = Linear Control
- 1 = Hybrid Predictive Control
- 2 = Full Adaptive Predictive Control

Recommended: Automatic.

------------------------------------------------------------------------

# Automatic Mode

## auto_linear_headroom

Thermal headroom threshold below which Linear Control is selected.

Typical values:

25--35°C

------------------------------------------------------------------------

## auto_hybrid_headroom

Thermal headroom threshold below which Hybrid Predictive Control is
selected.

Above this threshold Full Predictive Adaptive Predictive is selected.

Typical values:

50--60°C

------------------------------------------------------------------------

# Prediction Parameters

## prediction_horizon

Prediction horizon measured in controller cycles.

Higher values:

- Earlier prediction
- Lower overshoot

Lower values:

- Faster heating
- More aggressive behaviour

------------------------------------------------------------------------

## prediction_pre_target_window

Defines how early predictive unloading may begin.

Increase:

- Smoother transition

Decrease:

- Maximum heat-up performance

------------------------------------------------------------------------

## thermal_memory_weight

Relative contribution of stored PTC thermal energy.

Increase:

- Earlier unloading

Decrease:

- Later unloading

------------------------------------------------------------------------

## prediction_rate_gain

Weight applied to chamber heating rate.

Increase:

- More responsive prediction

Decrease:

- Behaviour closer to Linear Control

------------------------------------------------------------------------

## prediction_rate_deadband

Temperature changes smaller than this value are ignored.

Purpose:

Reduce sensor noise.

------------------------------------------------------------------------

## adaptive_curve_mode

Selects the nonlinear curve.

Values:

- 0 = Linear
- 1 = Quadratic
- 2 = Smoothstep

Recommended:

Smoothstep.

------------------------------------------------------------------------

## adaptive_min_progress

Minimum unloading progress once prediction becomes active.

Normally left at zero.

------------------------------------------------------------------------

## adaptive_max_pre_target_progress

Maximum unloading allowed before the chamber reaches the real target.

Recommended:

0.20 -- 0.40

Lower values preserve heating speed.

Higher values reduce overshoot.

------------------------------------------------------------------------

# Fan Parameters

## fan_heat_max

Maximum circulation fan speed during heat-up.

## fan_min

Minimum circulation fan speed.

## fan_max

Absolute maximum circulation fan speed.

## fan_gain

Scaling factor used during adaptive fan control.

------------------------------------------------------------------------

# Overshoot Cooling

## overshoot_cooling_delta

Temperature above target required to enable overshoot cooling.

## overshoot_cooling_release

Temperature below which overshoot cooling is released.

## exhaust_min

Minimum exhaust fan speed.

## exhaust_max

Maximum exhaust fan speed.

## exhaust_gain

Gain applied to exhaust modulation.

------------------------------------------------------------------------

# Cooldown

## cooldown_temp

Temperature below which cooldown ends.

## cooldown_fan

Fan speed during cooldown.

## exhaust_cooldown

Exhaust speed during cooldown.

------------------------------------------------------------------------

# Heat Assist Fans

## heat_assist_fans

Comma-separated list of heating-only assist fans.

## heat_assist_release_offset

Temperature above target at which assist fans are released for manual
control.

------------------------------------------------------------------------

# Internal Variables

Variables such as:

- last_chamber
- heater_alarm
- heater_soft_alarm
- chamber_alarm
- target_hold_alarm

are maintained automatically.

Users should not modify them manually.

------------------------------------------------------------------------

# Recommended Starting Configuration

Weak heater:

- Linear Control
- Prediction Horizon = 2
- Thermal Weight = 0.015

Medium heater:

- Hybrid Predictive

Powerful heater:

- Automatic Mode or Full Adaptive Predictive Control.

------------------------------------------------------------------------

# Final Notes

Tune only one parameter at a time.

Document every change.

Always verify safety behaviour after modifying thermal parameters.
