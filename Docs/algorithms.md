# Control Algorithms

This document explains every control algorithm implemented by Smart
Chamber Heater.

------------------------------------------------------------------------

# Overview

Smart Chamber Heater can operate using four modes:

  ------------------------------------------------------------------------

  Mode                           Description                                      Recommended for

  ------------ ------------------------- ---------------------------------

  Auto                          Automatically selects the                  Most users
                                     most suitable algorithm   

  Linear                        Maximum heating until                     Low-power heaters
  Control                      target                    

  Hybrid                        Linear heating with                            Most printers
  Predictive                  predictive unloading      
  Control                                

  Full                              Fully adaptive predictive                        High-power heaters
  Predictive                   control                                                
  Control                                
  (Adaptive Predictive)                                  

  ------------------------------------------------------------------------

------------------------------------------------------------------------

# Automatic Control

Automatic mode selects the algorithm according to the available thermal
headroom.

Thermal Headroom:

    Heater Maximum Temperature
    -
    Chamber Target

Small thermal headroom results in Linear Control, medium headroom
selects Hybrid Predictive Control and large headroom selects Full
Predictive Adaptive Predictive Control.

------------------------------------------------------------------------

# Linear Control

Linear Control is intentionally simple.

Below the chamber target:

- Heater target = Maximum allowed PTC temperature
- Chamber fan = Maximum heating speed

Once the chamber reaches the target, the controller progressively
unloads the heater until Hold Mode is reached.

Advantages:

- Fastest possible heat-up.
- Ideal for limited heaters.
- Very easy to tune.

Limitations:

- Does not anticipate overshoot.

------------------------------------------------------------------------

# Hybrid Predictive Control

Hybrid Predictive Control preserves the aggressive heat-up behaviour of
Linear Control while adding prediction only when it becomes useful.

The controller continuously computes:

- Current chamber temperature
- Chamber heating rate
- Remaining thermal energy inside the PTC

If the prediction indicates that the chamber is likely to exceed the
configured overshoot window, unloading begins before the real
temperature reaches that point.

Advantages:

- Excellent balance between speed and stability.
- Minimal configuration.
- Suitable for most printers.

------------------------------------------------------------------------

# Full Adaptive Predictive Control

Full Adaptive Predictive continuously adapts heater output according to the thermal
behaviour of the printer.

Inputs:

- Chamber temperature
- Chamber heating rate
- Remaining PTC thermal energy
- Prediction horizon
- Non linear curve

The prediction estimates future chamber temperature and the unloading
curve continuously adapts heater target and fan speed.

Advantages:

- Lowest overshoot.
- Smoothest thermal transition.
- Best stability on powerful heating systems.

Limitations:

- More parameters to tune.
- Less useful on heaters with very small thermal headroom.

------------------------------------------------------------------------

# Comparison

  Feature                                        Linear   Hybrid   Full Adaptive Predictive

  ----------------------------- -------- -------- ----------

  Current chamber temperature          ✓        ✓         ✓
  Chamber heating rate                         ✗        ✓         ✓
  PTC thermal memory                          ✗        ✓         ✓
  Future prediction                                 ✗        ✓         ✓
  Nonlinear curve                                   ✗        ✗         ✓

------------------------------------------------------------------------

# Which mode should I use?

Use Linear Control if the heater struggles to reach the chamber target.

Use Hybrid Predictive Control for most enclosed printers.

Use Full Predictive Adaptive Predictive when using a high-power heater with significant
thermal headroom where overshoot prevention becomes more important than
absolute heat-up speed.
