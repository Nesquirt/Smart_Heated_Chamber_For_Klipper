# Theory of Operation

## Introduction

Smart Chamber Heater was designed around a simple observation:

A heated chamber is **not** a thermostat.

It is a thermal system composed of multiple elements storing and
exchanging energy at different speeds.

Understanding these interactions is the foundation of the controller.

------------------------------------------------------------------------

# Thermal System

A typical enclosed printer contains several thermal masses:

-   PTC heater
-   Chamber air
-   Printed parts
-   Build plate
-   Frame and panels

Each element heats and cools at a different rate.

The controller therefore manages energy flow rather than simply
controlling temperature.

------------------------------------------------------------------------

# Why Overshoot Happens

Overshoot is not caused by inaccurate sensors.

It is caused by **stored thermal energy**.

When the PTC reaches a high temperature, it continues transferring heat
even after its target has been reduced.

The chamber therefore continues to warm for a period of time.

This delayed response is known as **thermal inertia**.

------------------------------------------------------------------------

# Thermal Inertia

The chamber temperature always lags behind the heater temperature.

Typical sequence:

1.  Heater temperature rises rapidly.
2.  Chamber temperature rises slowly.
3.  Heater target is reduced.
4.  Heater is still hotter than the chamber.
5.  Heat continues flowing.
6.  Chamber reaches its maximum temperature after the heater has already
    started cooling.

This delayed response is the main reason prediction improves
performance.

------------------------------------------------------------------------

# PTC Thermal Memory

Smart Chamber Heater models the heater as a temporary thermal energy
storage.

The controller estimates how much usable heat is still available by
comparing:

-   Current PTC temperature
-   Hold PTC temperature

The difference is called **PTC Thermal Memory**.

Large difference:

-   More residual heat available.

Small difference:

-   Little remaining energy.

------------------------------------------------------------------------

# Linear Control

Linear Control assumes only one relevant variable:

Current chamber temperature.

Advantages:

-   Maximum heating performance.
-   Very simple behaviour.

Limitations:

-   Cannot anticipate thermal inertia.

------------------------------------------------------------------------

# Hybrid Predictive Control

Hybrid Predictive Control introduces a prediction layer without changing
the overall heating strategy.

During most of the heat-up process the controller behaves like Linear
Control.

Only near the target does prediction influence the unloading process.

This provides excellent performance while remaining easy to tune.

------------------------------------------------------------------------

# Full Adaptive Predictive Control

Full Predictive Adaptive Predictive continuously adapts heater output.

The decision is based on multiple variables simultaneously:

-   Chamber temperature
-   Heating rate
-   Remaining PTC thermal energy
-   Prediction horizon

The relationship between these variables is nonlinear.

Different thermal conditions therefore produce different unloading
curves.

------------------------------------------------------------------------

# Prediction

The controller estimates future chamber temperature using:

Predicted Chamber

=

Current Chamber

-   

Heating Rate × Prediction Horizon

-   

PTC Thermal Memory

The objective is not perfect physical simulation.

The objective is to estimate enough future behaviour to improve
stability.

------------------------------------------------------------------------

# Why Not a Classical PID?

A PID controller only reacts to measured error.

It has no knowledge of:

-   Remaining heater energy
-   Future chamber behaviour
-   Thermal headroom

Smart Chamber Heater adds this missing information.

Prediction complements the heater PID instead of replacing it.

The heater PID still regulates heater temperature.

Smart Chamber Heater regulates **how the heater should be used**.

------------------------------------------------------------------------

# Thermal Headroom

Thermal Headroom measures the available temperature difference between
the heater and the requested chamber target.

Small headroom:

-   Maximum heating performance is more important than prediction.

Large headroom:

-   Prediction becomes increasingly beneficial.

This concept is the basis of Automatic Control Mode.

------------------------------------------------------------------------

# Design Principles

The controller was designed around five principles:

1.  Safety before performance.
2.  Adapt to different hardware.
3.  Keep configuration intuitive.
4.  Reduce unnecessary heater cycling.
5.  Improve chamber stability without sacrificing heat-up speed.

------------------------------------------------------------------------

# Future Evolution

Future versions may include:

-   Self-learning thermal models.
-   Automatic heater characterization.
-   Chamber thermal mass estimation.
-   Ambient temperature compensation.
-   Adaptive prediction using historical heating cycles.

These additions can further improve accuracy while preserving the
lightweight architecture required by Klipper macros.
