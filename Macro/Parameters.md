# ⚙️ Important Parameters

This file lists the most important parameters to configure in `chamber_heater.cfg`.

Smart Chamber Heater contains many variables, but most users only need to adjust a small set of them before first use.

> [!IMPORTANT]
>
> The values below are taken from the current `chamber_heater.cfg` configuration layout.
>
> Do not blindly copy values from other printers. Chamber volume, insulation, heater power, airflow and thermal fuse rating all matter.

---

# 🟢 Basic Parameters

These are the first parameters most users should check.

## `variable_heater_fan_name`

Name of the fan used to move air through or around the chamber heater.

```cfg
variable_heater_fan_name: "chamber_heater_fan"
```

This must match the name of the related `[fan_generic ...]` section.

## `variable_exhaust_fan_name`

Name of the exhaust fan used for cooldown and overshoot control.

```cfg
variable_exhaust_fan_name: "Exhaust"
```

## `variable_heat_assist_fans`

Comma-separated list of assist fans used only during the heat-up phase.

```cfg
variable_heat_assist_fans: "RSCS"
```

Examples:

```cfg
variable_heat_assist_fans: "RSCS"
variable_heat_assist_fans: "RSCS,Fan2"
variable_heat_assist_fans: ""
```

If you do not use assist fans, leave it empty.

## `variable_heat_assist_release_offset`

Defines when heat-assist fans are released.

```cfg
variable_heat_assist_release_offset: 0.2
```

Meaning:

```text
Release assist fans at TARGET + 0.2°C
```

---

# 🌡️ Safety and Temperature Limits

These values are critical. Set them according to your real hardware.

## `variable_thermal_fuse_temp`

Real temperature rating of the thermal fuse installed on the PTC heater.

```cfg
variable_thermal_fuse_temp: 85
```

> [!WARNING]
>
> This value must match the real installed thermal fuse.
>
> Do not increase it unless the thermal fuse has physically been replaced.

The macro automatically calculates several PTC safety limits from this value:

```text
heater_max        = thermal_fuse_temp - 10
heater_soft_limit = thermal_fuse_temp - 7
heater_safety     = thermal_fuse_temp - 5
```

Example with an 85°C thermal fuse:

```text
heater_max        = 75°C
heater_soft_limit = 78°C
heater_safety     = 80°C
```

## `variable_max_chamber_target`

Maximum chamber temperature the user is allowed to request.

```cfg
variable_max_chamber_target: 75
```

This is the maximum **chamber target**, not the PTC temperature.

---

# 🔥 Heating Behaviour

## `variable_heater_min`

Minimum PTC target allowed during active control.

```cfg
variable_heater_min: 45
```

## `variable_heating_cutoff_offset`

Defines when the controller enters target hold mode.

```cfg
variable_heating_cutoff_offset: 0.3
```

Meaning:

```text
Enter hold mode at TARGET + 0.3°C
```

## `variable_heating_resume_offset`

Defines when the controller exits hold mode and resumes heat-up control.

```cfg
variable_heating_resume_offset: 0.5
```

Meaning:

```text
Resume heating at TARGET - 0.5°C
```

---

# 🧊 Hold Mode

## `variable_hold_enabled`

Enables or disables Hold Mode.

```cfg
variable_hold_enabled: 1
```

| Value | Meaning |
|------:|---------|
| `0` | Hold Mode disabled |
| `1` | Hold Mode enabled |

## `variable_hold_ptc_delta`

PTC temperature offset used during Hold Mode.

```cfg
variable_hold_ptc_delta: 5
```

Meaning:

```text
Hold PTC target = chamber target + hold_ptc_delta
```

Example:

```text
Chamber target = 50°C
hold_ptc_delta = 5°C
Hold PTC target = 55°C
```

Increase if the chamber slowly drops below target.

Decrease if the chamber overshoots during hold.

## `variable_hold_fan_speed`

Heater fan speed during Hold Mode.

```cfg
variable_hold_fan_speed: 0.65
```

Range:

```text
0.0 = off
1.0 = 100%
```

---

# 🧠 Control Algorithm

## `variable_control_mode`

Selects the active control algorithm.

```cfg
variable_control_mode: 1
```

| Value | Mode |
|------:|------|
| `-1` | Automatic Control Selection |
| `0` | Linear Control |
| `1` | Hybrid Predictive Control |
| `2` | Full Adaptive Predictive Control |

Recommended starting value:

```cfg
variable_control_mode: 1
```

Use `-1` if you want the macro to select the mode automatically from thermal headroom.

## `variable_auto_linear_headroom`

Thermal headroom threshold for Linear Control in Auto Mode.

```cfg
variable_auto_linear_headroom: 25
```

Thermal headroom is calculated as:

```text
HEATER_MAX - chamber target
```

## `variable_auto_hybrid_headroom`

Thermal headroom threshold for Hybrid Predictive Control in Auto Mode.

```cfg
variable_auto_hybrid_headroom: 50
```

Auto Mode logic:

```text
headroom <= auto_linear_headroom
    Linear Control

headroom <= auto_hybrid_headroom
    Hybrid Predictive Control

headroom > auto_hybrid_headroom
    Full Adaptive Predictive Control
```

---

# 🔮 Prediction Parameters

Most users should keep these defaults at first.

## `variable_prediction_horizon`

Prediction horizon expressed in chamber loop cycles.

```cfg
variable_prediction_horizon: 1.0
```

Higher values predict earlier and reduce overshoot more aggressively.

Lower values keep heat-up more aggressive.

## `variable_prediction_pre_target_window`

How early the predictive ramp may begin before the real chamber target.

```cfg
variable_prediction_pre_target_window: 0.3
```

Weak heaters:

```text
0.2 - 0.5°C
```

Powerful heaters:

```text
1.0 - 2.0°C
```

## `variable_thermal_memory_weight`

Weight of the residual heat stored in the PTC.

```cfg
variable_thermal_memory_weight: 0.015
```

Higher values unload earlier.

Lower values keep more heat.

## `variable_prediction_rate_gain`

Gain applied to chamber heating rate.

```cfg
variable_prediction_rate_gain: 1.0
```

Higher values react more strongly to fast chamber rise.

## `variable_prediction_rate_deadband`

Small chamber temperature changes below this value are ignored.

```cfg
variable_prediction_rate_deadband: 0.02
```

This helps filter sensor noise.

---

# 📈 Adaptive Curve Parameters

## `variable_adaptive_curve_mode`

Selects the adaptive unloading curve.

```cfg
variable_adaptive_curve_mode: 2
```

| Value | Curve |
|------:|-------|
| `0` | Linear ramp |
| `1` | Quadratic ease-in |
| `2` | Smoothstep S-curve |

Recommended:

```cfg
variable_adaptive_curve_mode: 2
```

## `variable_adaptive_min_progress`

Minimum predictive ramp progress once prediction enters the control window.

```cfg
variable_adaptive_min_progress: 0.0
```

Usually leave this at `0.0`.

## `variable_adaptive_max_pre_target_progress`

Maximum unloading allowed before the real chamber reaches target.

```cfg
variable_adaptive_max_pre_target_progress: 0.35
```

Meaning:

```text
0.35 = max 35% of the unloading ramp before the chamber physically reaches target
```

Increase for powerful heaters.

Decrease for weak heaters.

---

# 🌬️ Fan Parameters

## `variable_fan_min`

Minimum heater fan speed.

```cfg
variable_fan_min: 0.75
```

## `variable_fan_heat_max`

Maximum heater fan speed during heat-up.

```cfg
variable_fan_heat_max: 1.0
```

## `variable_fan_max`

Absolute maximum heater fan speed.

```cfg
variable_fan_max: 1.0
```

## `variable_fan_gain`

Fan gain used by adaptive fan logic.

```cfg
variable_fan_gain: 0.03
```

Most users should keep this unchanged.

---

# 🌀 Cooldown Parameters

## `variable_cooldown_temp`

Temperature below which cooldown ends.

```cfg
variable_cooldown_temp: 35
```

## `variable_cooldown_fan`

Chamber fan speed during cooldown.

```cfg
variable_cooldown_fan: 1.0
```

## `variable_exhaust_cooldown`

Exhaust fan speed during cooldown.

```cfg
variable_exhaust_cooldown: 1.0
```

---

# 💨 Overshoot Cooling

## `variable_overshoot_cooling_delta`

Overshoot required to activate exhaust cooling.

```cfg
variable_overshoot_cooling_delta: 1.5
```

Meaning:

```text
Activate overshoot cooling at TARGET + 1.5°C
```

## `variable_overshoot_cooling_release`

Temperature offset used to release overshoot cooling.

```cfg
variable_overshoot_cooling_release: 0.2
```

## `variable_exhaust_min`

Minimum exhaust fan speed during modulated overshoot cooling.

```cfg
variable_exhaust_min: 0.35
```

## `variable_exhaust_max`

Maximum exhaust fan speed.

```cfg
variable_exhaust_max: 1.0
```

## `variable_exhaust_gain`

Gain used to modulate exhaust speed during overshoot cooling.

```cfg
variable_exhaust_gain: 0.20
```

---

# 🔧 Heater Hardware Section

These values are in `[heater_generic chamber_heater]`.

They must be adapted to your hardware.

## `heater_pin`

Output pin controlling the PTC heater.

```cfg
heater_pin: PF6
```

## `sensor_type`

PTC temperature sensor type.

```cfg
sensor_type: Generic 3950
```

## `sensor_pin`

PTC temperature sensor pin.

```cfg
sensor_pin: PF9
```

## `control`

Heater control method.

```cfg
control: pid
```

| Switching hardware | Recommended control |
|-------------------|---------------------|
| SSR / MOSFET | `pid` |
| Mechanical relay | `watermark` |

## PID values

```cfg
pid_Kp: 18.734
pid_Ki: 1.201
pid_Kd: 73.062
```

Run a dedicated PID calibration and copy the generated values manually into this section.

## `max_power`

Maximum heater output power.

```cfg
max_power: 0.50
```

## `max_temp`

Maximum allowed PTC sensor temperature for Klipper.

```cfg
max_temp: 90
```

---

# ✅ Minimal First Configuration

For a first setup, check only these values:

```cfg
variable_heater_fan_name: "chamber_heater_fan"
variable_exhaust_fan_name: "Exhaust"
variable_heat_assist_fans: "RSCS"

variable_thermal_fuse_temp: 85
variable_max_chamber_target: 75

variable_control_mode: 1

heater_pin: PF6
sensor_pin: PF9
sensor_type: Generic 3950

control: pid
pid_Kp: <your calibrated value>
pid_Ki: <your calibrated value>
pid_Kd: <your calibrated value>
```

After these are correct, tune the advanced prediction and fan parameters only if needed.
