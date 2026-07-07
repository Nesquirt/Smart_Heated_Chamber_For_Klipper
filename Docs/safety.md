# Safety System

## Introduction

Safety is the highest priority within Smart Chamber Heater.

All protection mechanisms are independent from the selected control
algorithm.

Whether the controller is operating in Linear, Hybrid Predictive or Full
Predictive Adaptive Predictive mode, safety logic always overrides heating commands.

------------------------------------------------------------------------

# Safety Priority

The controller evaluates protections in the following order:

1.  Chamber Overtemperature
2.  Overshoot Cooling
3.  PTC Emergency Protection
4.  PTC Soft Limit
5.  Hold Mode
6.  Normal Heating Control

This guarantees that thermal protection always has precedence over
performance.

------------------------------------------------------------------------

# Thermal Fuse Protection

The thermal fuse represents the absolute hardware limit.

The variable:

`thermal_fuse_temp`

must always match the rating of the installed thermal fuse.

The controller automatically derives:

-   Maximum PTC temperature
-   Soft limit
-   Soft release
-   Safety shutdown threshold
-   Recovery threshold

Do **not** configure temperatures above the real hardware limit.

------------------------------------------------------------------------

# PTC Maximum Temperature

Maximum operating temperature is automatically calculated.

This leaves a safety margin below the thermal fuse to avoid nuisance
trips and improve hardware reliability.

------------------------------------------------------------------------

# Soft Limit Protection

When the PTC approaches its configured operating limit, the controller
does not immediately shut the heater down.

Instead it temporarily reduces the heater target.

Advantages:

-   smoother operation
-   reduced oscillation
-   improved hardware lifetime

Once the heater cools below the release threshold, normal operation
automatically resumes.

------------------------------------------------------------------------

# Emergency Heater Protection

If the heater reaches the emergency threshold:

-   PTC output is disabled
-   Chamber circulation fan switches to maximum
-   Exhaust fan switches to maximum
-   Assist fans are disabled

Heating resumes only after the heater has cooled below the configured
recovery threshold.

------------------------------------------------------------------------

# Chamber Overtemperature Protection

If the chamber exceeds the allowed safety temperature:

-   heater is switched off
-   circulation fan runs at maximum
-   exhaust fan runs at maximum

This protection is independent of the selected chamber target.

------------------------------------------------------------------------

# Overshoot Cooling

Overshoot Cooling is not a fault condition.

It is an automatic stabilization feature.

If the chamber temperature exceeds the configured overshoot window:

-   heater output is disabled
-   exhaust fan speed is automatically modulated
-   circulation fan remains active

Once the chamber returns inside the release window, normal operation
resumes automatically.

------------------------------------------------------------------------

# Hold Mode Safety

Hold Mode never bypasses safety limits.

Even while maintaining chamber temperature:

-   soft limits remain active
-   emergency protection remains active
-   chamber protection remains active

------------------------------------------------------------------------

# Automatic Cooldown

When chamber heating is disabled:

-   heater output is turned off
-   circulation fan continues running
-   exhaust fan continues running

Cooldown ends automatically once the heater temperature drops below the
configured cooldown temperature.

------------------------------------------------------------------------

# Heat Assist Fan Management

Assist fans are only controlled during the heat-up phase.

Once released, they become available for manual control or external
macros.

This prevents conflicts with user-defined workflows while preserving
safe heating behaviour.

------------------------------------------------------------------------

# Diagnostic Commands

The following commands can be used to verify the protection system:

  Macro                    Purpose
  ------------------------ --------------------------------------
  CHAMBER_LIMITS           Display calculated safety thresholds
  CHAMBER_CONTROL_STATUS   Display current controller state
  CHAMBER_PREDICTION_STATUS       Display prediction variables

------------------------------------------------------------------------

# Best Practices

-   Always install a correctly rated thermal fuse.
-   Verify temperature sensor readings before first use.
-   Never increase safety temperatures to compensate for insufficient
    heater power.
-   Test protections after major hardware modifications.
-   Keep airflow paths unobstructed.

------------------------------------------------------------------------

# Design Philosophy

Smart Chamber Heater treats safety as a completely independent
subsystem.

Control algorithms may change in future releases, but protection
mechanisms are designed to remain deterministic, predictable and always
take priority over performance.
