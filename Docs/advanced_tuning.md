# Advanced Tuning Guide

## Purpose

This guide is intended for advanced users who want to extract the
maximum performance from Smart Chamber Heater.

Unlike the basic tuning guide, this document focuses on understanding
the thermal behaviour of the printer and selecting the most appropriate
control strategy.

------------------------------------------------------------------------

# Step 1 - Characterize the Heater

Before tuning the controller, understand your heater.

Measure:

- Maximum reachable PTC temperature
- Chamber heat-up time
- Stable chamber temperature
- Heater cooldown time

These measurements determine the available thermal headroom.

------------------------------------------------------------------------

# Step 2 - Characterize the Chamber

Record a complete heating cycle.

Observe:

- Chamber temperature
- Heater temperature
- Time to target
- Overshoot
- Stabilization time

Repeat the test at least three times.

------------------------------------------------------------------------

# Understanding Thermal Headroom

Thermal Headroom is the most important indicator for selecting the
control algorithm.

Small headroom:

- Heater operates close to chamber target.
- Maximum heating performance is required.
- Linear Control is usually preferred.

Large headroom:

- Heater stores much more thermal energy.
- Prediction becomes increasingly beneficial.

------------------------------------------------------------------------

# Reading Heating Curves

## Ideal Curve

Characteristics:

- Fast initial rise
- Smooth approach
- Minimal overshoot
- Stable Hold Mode

------------------------------------------------------------------------

## Slow Heating

Symptoms:

- Long heat-up time
- Little or no overshoot

Possible causes:

- Heater undersized
- Chamber poorly insulated
- Hold temperature too low

Recommended actions:

- Linear Control
- Lower prediction aggressiveness
- Improve chamber insulation

------------------------------------------------------------------------

## Aggressive Heating

Symptoms:

- Very fast heat-up
- Significant overshoot

Recommended actions:

- Hybrid Predictive Control
- Full Predictive Adaptive Predictive
- Increase prediction horizon

------------------------------------------------------------------------

# Choosing the Right Algorithm

## Linear Control

Choose when:

- Heater struggles to reach target
- Chamber is very large
- Heat-up time is the priority

------------------------------------------------------------------------

## Hybrid Predictive Control

Choose when:

- Heater reaches target comfortably
- Moderate overshoot is observed
- Balanced behaviour is desired

------------------------------------------------------------------------

## Full Adaptive Predictive Control

Choose when:

- Heater is significantly more powerful than required
- Overshoot dominates system behaviour
- Maximum stability is required

------------------------------------------------------------------------

# Typical Heater Categories

## Low Thermal Headroom

Examples:

- 75°C maximum heater
- Large enclosed printers

Recommended:

Linear Control

------------------------------------------------------------------------

## Medium Thermal Headroom

Examples:

- 90--100°C heater

Recommended:

Hybrid Predictive Control

------------------------------------------------------------------------

## High Thermal Headroom

Examples:

- 120--130°C heater

Recommended:

Full Adaptive Predictive Control

------------------------------------------------------------------------

# Test Procedure

After every configuration change:

1. Start from room temperature.
2. Heat the chamber.
3. Record:
   - Time to target
   - Maximum overshoot
   - Stabilization time
4. Repeat at least three times.

Never tune using a single heating cycle.

------------------------------------------------------------------------

# Suggested Optimization Order

1. Safety limits
2. Hold Mode
3. Heater fan
4. Prediction Horizon
5. Thermal Weight
6. Rate Gain
7. Curve selection
8. Auto Mode thresholds

------------------------------------------------------------------------

# Performance Metrics

Useful values to compare configurations:

- Time to target
- Maximum overshoot
- Hold stability
- Heater duty cycle
- Fan duty cycle
- Number of heater cycles

Keeping a tuning log is highly recommended.

------------------------------------------------------------------------

# Engineering Philosophy

Do not tune for the fastest possible heat-up.

Tune for the fastest heat-up that still reaches a stable chamber with
minimal overshoot and repeatable behaviour.

The best controller is not the most aggressive one.

It is the one that consistently produces the same thermal conditions
before every print.
