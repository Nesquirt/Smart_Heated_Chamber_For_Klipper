# Automatic Control Mode

## Overview

Automatic Control Mode allows Smart Chamber Heater to select the most
appropriate control algorithm without user intervention.

Instead of forcing a specific strategy, the controller evaluates the
available thermal headroom and automatically chooses the algorithm that
best matches the thermal characteristics of the printer.

------------------------------------------------------------------------

# What is Thermal Headroom?

Thermal Headroom is the difference between the maximum usable PTC
temperature and the requested chamber target.

    Thermal Headroom = Heater Maximum Temperature - Chamber Target

Example:

- Thermal fuse: 85°C
- Maximum PTC temperature: 75°C
- Chamber target: 50°C

Thermal Headroom = 25°C

------------------------------------------------------------------------

# Selection Logic

When `control_mode = -1`, the controller performs the following
evaluation:

1. Calculate Thermal Headroom.
2. Compare it with the configured thresholds.
3. Select the most suitable control algorithm.

Default behaviour:

                    Thermal Headroom Selected Algorithm

  ---------------------------------- -----------------------------------

             ≤ Auto Linear Threshold Linear Control
    \> Linear and ≤ Hybrid Threshold Hybrid Predictive Control
                 \> Hybrid Threshold Full Adaptive Predictive Control

------------------------------------------------------------------------

# Why This Works

Not every printer behaves the same.

A heater capable of reaching very high temperatures stores much more
thermal energy than a heater operating close to the chamber target.

Using the same algorithm for both systems would either:

- reduce heating performance on weak heaters, or
- increase overshoot on powerful heaters.

Automatic mode solves this problem by adapting the control strategy to
the available thermal energy.

------------------------------------------------------------------------

# Configuration

The following variables control Automatic Mode:

    variable_control_mode: -1
    
    variable_auto_linear_headroom: 30
    
    variable_auto_hybrid_headroom: 55

These values can be tuned according to the printer.

Lower thresholds make the controller select predictive algorithms
earlier.

Higher thresholds favour Linear Control.

------------------------------------------------------------------------

# Typical Examples

## Small PTC

Maximum PTC: 75°C

Target: 50°C

Headroom: 25°C

Selected algorithm:

**Linear Control**

------------------------------------------------------------------------

## Medium Power Heater

Maximum PTC: 90°C

Target: 50°C

Headroom: 40°C

Selected algorithm:

**Hybrid Predictive Control**

------------------------------------------------------------------------

## High Power Heater

Maximum PTC: 125°C

Target: 50°C

Headroom: 75°C

Selected algorithm:

**Full Adaptive Predictive Control**

------------------------------------------------------------------------

# Manual Override

Automatic Mode can always be overridden.

    0 = Linear Control
    
    1 = Hybrid Predictive Control
    
    2 = Full Adaptive Predictive Control

This is useful for testing and benchmarking different strategies.

------------------------------------------------------------------------

# Future Improvements

Future versions may extend Automatic Mode by considering:

- Average chamber heating rate
- Thermal inertia
- Heater efficiency
- Chamber volume estimation
- Adaptive self-learning

This would allow the controller to choose the algorithm based not only
on heater capability but also on the real thermal behaviour of the
printer.

------------------------------------------------------------------------

# Design Philosophy

Automatic Mode is intended to provide sensible defaults for most users
while keeping full manual control available for advanced tuning.

It is designed to make Smart Chamber Heater portable across many
different printer configurations with minimal user intervention.
