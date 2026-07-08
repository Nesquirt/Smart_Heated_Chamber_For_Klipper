# 📖 Usage

Using Smart Chamber Heater is designed to be as simple as using any standard Klipper heater.

Follow these **5 simple steps** and let the controller manage the rest automatically.

---

# 🚀 Step 1 — Install Smart Chamber Heater

Copy `chamber_heater.cfg` into your Klipper configuration directory.

Add the following line to your `printer.cfg`:

```cfg
[include chamber_heater.cfg]
```

Save the file and restart Klipper.

Once Klipper has restarted successfully, Smart Chamber Heater is ready to use.

---

# 🌡️ Step 2 — Set Your Desired Chamber Temperature

Use the `chamber_heater` temperature control in Fluidd, Mainsail or via G-code.

Example:

```gcode
SET_HEATER_TEMPERATURE HEATER=chamber_heater TARGET=50
```

or simply enter **50°C** in the web interface.

Nothing else needs to be configured during normal operation.

---

# ❗ Step 3 — Understand What You're Setting

> [!IMPORTANT]
>
> The value entered into `chamber_heater` is the **desired chamber temperature**, **not** the temperature of the PTC heater.

Examples:

| Desired chamber temperature | Set |
|----------------------------:|:---:|
| 40°C | 40°C |
| 50°C | 50°C |
| 60°C | 60°C |

Do **not** enter the maximum operating temperature of your PTC heater.

If your heater can reach **75°C**, **90°C** or **125°C**, those are hardware limits—not the value you should normally enter.

For example:

```gcode
SET_HEATER_TEMPERATURE HEATER=chamber_heater TARGET=75
```

means:

> **"Heat the printer chamber to 75°C."**

It does **not** mean:

> **"Limit the PTC heater to 75°C."**

If you enter **75°C**, Smart Chamber Heater will attempt to heat the **entire chamber** to **75°C**.

---

# 🧠 Step 4 — Let the Controller Work

As soon as heating starts, Smart Chamber Heater stores your requested chamber temperature internally.

From that moment, it automatically manages the heater by continuously calculating the optimal heater target using:

- Chamber temperature
- Chamber heating rate
- Remaining thermal energy inside the PTC
- Selected control mode
- Hold Mode
- Safety limits
- Adaptive Predictive Control

Because of this, the displayed heater target will change during operation.

> [!NOTE]
>
> This is completely normal.
>
> The displayed heater target is a **temporary working value** calculated by the controller.
>
> Your original chamber temperature is always stored internally and never changes.

Example:

```
Requested chamber temperature
50°C

↓

Temporary heater targets

75°C
72°C
68°C
64°C
60°C
56°C

↓

Final chamber temperature

50°C
```

---

# 🤖 Step 5 — Enjoy Automatic Control

After selecting the desired chamber temperature, Smart Chamber Heater automatically manages:

- Heater target
- Fan speeds
- Hold Mode
- Overshoot mitigation
- Adaptive Predictive Control
- Automatic Control Mode (when enabled)

No further user interaction is required.

Simply choose the chamber temperature you want and let Smart Chamber Heater handle everything else.

> [!TIP]
>
> During normal operation, **do not manually change the heater target**.
>
> Dynamic heater target adjustments are an intentional part of the control strategy and are essential for achieving the best balance between heating speed, temperature stability and safety.
