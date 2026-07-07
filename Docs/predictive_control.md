# Predictive Control

## Introduction

Traditional chamber controllers only react to the current chamber
temperature.

Smart Chamber Heater can also estimate what will happen **before** the
chamber actually reaches the requested target.

This predictive approach allows the controller to reduce overshoot while
maintaining fast heat-up performance.

------------------------------------------------------------------------

# Why Prediction?

The chamber temperature always lags behind the heater.

Even if the PTC is switched off immediately, it still contains thermal
energy that will continue heating the chamber.

Prediction estimates this future behaviour.

------------------------------------------------------------------------

# Prediction Model

The simplified model used by Smart Chamber Heater is:

    Predicted Chamber Temperature =

    Current Chamber Temperature

    +

    (Chamber Heating Rate × Prediction Horizon × Rate Gain)

    +

    (Remaining PTC Thermal Energy × Thermal Weight)

Although intentionally lightweight, this model captures the most
important dynamics of a chamber heater.

------------------------------------------------------------------------

# Chamber Heating Rate

The controller measures the chamber temperature every control cycle.

    Heating Rate =
    Current Chamber
    -
    Previous Chamber

Positive values indicate heating.

Near-zero values indicate a stable chamber.

Negative values indicate cooling.

A configurable deadband removes sensor noise.

------------------------------------------------------------------------

# Prediction Horizon

Prediction Horizon defines how many future control cycles are estimated.

Small values:

-   Faster response
-   Less anticipation
-   More aggressive heating

Large values:

-   Earlier unloading
-   Better overshoot control
-   Slower final approach

------------------------------------------------------------------------

# PTC Thermal Memory

The PTC does not instantly stop transferring heat when its target
changes.

The controller estimates this remaining thermal energy by comparing:

-   Current PTC temperature
-   Hold PTC temperature

The larger the difference, the more heat is still available.

------------------------------------------------------------------------

# Hybrid Predictive Control

Hybrid mode behaves like Linear Control during most of the heat-up.

Prediction only influences the controller when it indicates that the
chamber may exceed the configured overshoot window.

This preserves maximum heating performance while avoiding unnecessary
overshoot.

------------------------------------------------------------------------

# Full Adaptive Predictive Control

Full Adaptive Predictive uses the prediction continuously.

The unloading curve begins before the chamber reaches the target and is
shaped using configurable nonlinear curves.

Available curve types:

-   Linear
-   Quadratic
-   Smoothstep (recommended)

------------------------------------------------------------------------

# Practical Example

Target: 50°C

Current chamber: 49.8°C

Heating rate: +0.20°C/cycle

PTC still significantly above Hold temperature.

Prediction estimates:

50.4°C

Rather than waiting until the chamber physically reaches 50.4°C, the
controller begins unloading earlier, reducing overshoot while
maintaining stability.

------------------------------------------------------------------------

# Tuning Tips

Increase:

-   Prediction Horizon
-   Rate Gain
-   Thermal Weight

to reduce overshoot.

Decrease them to maximize heat-up speed.

------------------------------------------------------------------------

# Design Philosophy

Prediction is intentionally simple.

The goal is not to build a full physical simulator but to estimate
enough future behaviour to improve thermal stability while remaining
computationally lightweight and easy to tune inside Klipper macros.
