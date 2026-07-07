# Tuning Guide

## Introduction

This guide explains how to tune Smart Chamber Heater for the best
balance between heating speed, temperature stability and overshoot
prevention.

No single configuration fits every printer. The correct settings depend
on:

- Heater power
- Chamber size
- Airflow
- Ambient temperature
- Thermal insulation

------------------------------------------------------------------------

------------------------------------------------------------------------

------------------------------------------------------------------------

# Before Tuning: Run a Dedicated Heater PID Calibration

Before changing any Smart Chamber Heater control parameter, the PTC heater PID must be calibrated separately.

The chamber controller assumes that the PTC heater itself is already stable and correctly tuned.  
If the heater PID is wrong, every higher-level feature such as Hold Mode, predictive unloading and Full Adaptive Predictive will behave unpredictably.

## Important note about SAVE_CONFIG

If the chamber heater is defined inside a separate included configuration file, Klipper may not write the generated PID values back into that file automatically.

For this reason, the recommended method is:

1. Run the PID calibration macro.
2. Read the generated PID values from the console.
3. Copy the values manually.
4. Paste them into the `[heater_generic chamber_heater]` section.
5. Restart Klipper.

## PID helper macro

Smart Chamber Heater can include the following helper command:

```gcode
CHAMBER_PID_CALIBRATE TARGET=75
```

This command runs:

```gcode
PID_CALIBRATE HEATER=chamber_heater TARGET=75
```

and then prompts the user to copy the generated values manually.

Use a target close to the real operating temperature of the PTC heater.

Recommended examples:

| Heater limit | Suggested PID target |
| ------------:| --------------------:|
| 75°C max     | 70–75°C              |
| 90°C max     | 80–85°C              |
| 125°C max    | 100–110°C            |

## Manual copy location

After calibration, copy the generated values into:

```cfg
[heater_generic chamber_heater]
control: pid
pid_Kp: <generated value>
pid_Ki: <generated value>
pid_Kd: <generated value>
```

Then restart Klipper.

Do not tune predictive control or Hold Mode before the heater PID has been calibrated.

# Safety First

Before changing any control parameter verify:

- `thermal_fuse_temp`
- `max_chamber_target`

Never configure temperatures above the real hardware limits.

------------------------------------------------------------------------

# Hold Mode

## Chamber oscillates after reaching target

Increase:

- `hold_ptc_delta`

This stores more thermal energy inside the heater.

------------------------------------------------------------------------

## Chamber slowly cools below target

Increase:

- `hold_ptc_delta`
- `hold_fan_speed`

------------------------------------------------------------------------

## Chamber overshoots while holding

Decrease:

- `hold_ptc_delta`

or

- `hold_fan_speed`

------------------------------------------------------------------------

# Prediction Parameters

## Prediction Horizon

`prediction_horizon`

Increase:

- earlier unloading
- lower overshoot

Decrease:

- faster heat-up
- more aggressive behaviour

Recommended:

  Heater       Value

  ---------- -------

  Weak          1--2
  Medium        2--3
  Powerful      3--5

------------------------------------------------------------------------

## Thermal Weight

`thermal_memory_weight`

Controls how much remaining PTC heat contributes to prediction.

Increase:

- unload sooner

Decrease:

- keep heating longer

Recommended:

0.010 -- 0.030

------------------------------------------------------------------------

## Rate Gain

`prediction_rate_gain`

Controls how strongly the chamber heating rate affects prediction.

Increase if:

- chamber heats very quickly

Decrease if:

- controller unloads too early

------------------------------------------------------------------------

## Prediction Window

`prediction_pre_target_window`

Defines how early Full Predictive Control begins acting.

Small values:

- aggressive heating

Large values:

- smoother approach

------------------------------------------------------------------------

# Heating Curves

## Linear

Best for weak heaters.

------------------------------------------------------------------------

## Quadratic

Keeps maximum power for longer.

Suitable for medium heaters.

------------------------------------------------------------------------

## Smoothstep

Recommended default.

Provides smooth transitions and excellent stability.

------------------------------------------------------------------------

# Auto Mode Thresholds

`auto_linear_headroom`

Increase if:

- Linear Control should remain active on more printers.

Decrease if:

- predictive control should activate sooner.

------------------------------------------------------------------------

`auto_hybrid_headroom`

Increase if:

- Hybrid should remain active longer.

Decrease if:

- Full Adaptive Predictive should activate sooner.

------------------------------------------------------------------------

# Troubleshooting

## Chamber heats too slowly

Possible causes:

- Heater too small
- Hold temperature too low
- Prediction too aggressive

Try:

- Linear Control
- Lower prediction horizon
- Lower thermal weight

------------------------------------------------------------------------

## Chamber overshoots

Try:

- Increase prediction horizon
- Increase thermal weight
- Increase pre-target window

------------------------------------------------------------------------

## Heater unloads too early

Decrease:

- prediction horizon
- rate gain
- thermal weight

------------------------------------------------------------------------

## Heater unloads too late

Increase:

- prediction horizon
- rate gain
- thermal weight

------------------------------------------------------------------------

# Example Configurations

## Small Heater (75°C max)

Recommended:

- Linear Control
- Prediction Horizon: 2
- Thermal Weight: 0.015

------------------------------------------------------------------------

## Medium Heater (90°C max)

Recommended:

- Hybrid Predictive
- Prediction Horizon: 2--3
- Thermal Weight: 0.020

------------------------------------------------------------------------

## Powerful Heater (125°C +)

Recommended:

- Full Predictive Adaptive Predictive
- Prediction Horizon: 3--4
- Thermal Weight: 0.025--0.030

------------------------------------------------------------------------

# Final Advice

Only change one parameter at a time.

Allow the chamber to complete at least one full heating cycle before
evaluating the effect of each adjustment.

Keeping notes of every change makes tuning significantly easier and
helps identify the optimal configuration for your printer.
