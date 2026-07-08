# 📖 Usage

Using Smart Chamber Heater is intentionally simple.

Unlike traditional chamber controllers, **you only need to choose the chamber temperature you want**. Smart Chamber Heater automatically manages everything else.

---

# 🚀 Step 1 — Install the Controller

Copy `chamber_heater.cfg` into your Klipper configuration directory.

Add the following line to your `printer.cfg`:

```cfg
[include chamber_heater.cfg]
```

Save the file and restart Klipper.

Once Klipper has restarted successfully, Smart Chamber Heater is ready to use.

---

# 🌡️ Step 2 — Choose Your Chamber Temperature

Set the desired chamber temperature using the standard `chamber_heater` control from Fluidd, Mainsail or through G-code.

For example:

```text
SET_HEATER_TEMPERATURE HEATER=chamber_heater TARGET=50
```

or simply enter **50°C** in the `chamber_heater` temperature box.

At this point, you are **NOT** setting the temperature of the PTC heater.

Instead, you are telling Smart Chamber Heater:

> **"I want my printer chamber to reach 50°C."**

That's all.

From now on, Smart Chamber Heater will handle the entire heating process automatically.

---

# ❗ Step 3 — Don't Be Surprised When the Target Changes

This is the part that surprises almost every new user.

A few seconds after heating starts, you will notice that the displayed `chamber_heater` target begins changing automatically.

For example, you initially set:

```text
50°C
```

After a short time, you may see:

```text
75°C
72°C
68°C
64°C
60°C
56°C
```

This is **NOT** a bug.

It is **NOT** changing your desired chamber temperature.

It is simply Smart Chamber Heater taking control of the heater.

---

# 🧠 Step 4 — What Is Actually Happening?

As soon as heating begins, Smart Chamber Heater stores the value you requested.

For example:

```text
User request

50°C
```

Internally, the controller permanently stores:

```text
Desired chamber temperature

50°C
```

This value **never changes**.

It remains the target for the entire heating cycle.

Instead of keeping the heater at a fixed temperature, Smart Chamber Heater continuously calculates the best heater target using:

- Current chamber temperature
- Chamber heating rate
- Remaining thermal energy stored inside the PTC heater
- Selected control mode
- Hold Mode
- Safety limits
- Adaptive Predictive Control

The result is something like this:

```text
Your requested chamber temperature

50°C
        │
        ▼
Stored internally
50°C
        │
        ▼
Smart Chamber Heater
calculates the optimal
heater target
        │
        ▼
75°C
72°C
68°C
64°C
60°C
56°C
        │
        ▼
Measured chamber temperature

50°C
```

The changing values you see are **temporary heater targets**.

They are generated automatically to:

- Heat the chamber faster.
- Reduce overshoot.
- Improve temperature stability.
- Reduce unnecessary heater cycling.
- Prepare Hold Mode.
- Optimize Adaptive Predictive Control.

Your requested chamber temperature **always remains exactly the same**.

---

# 🤖 Step 5 — Let Smart Chamber Heater Do the Work

Once you've selected the desired chamber temperature, there is nothing else to do.

The controller automatically manages:

- Heater target
- Fan speeds
- Hold Mode
- Adaptive Predictive Control
- Overshoot mitigation
- Automatic Control Mode (if enabled)

Simply choose the chamber temperature you want and let Smart Chamber Heater handle the rest.

---

> [!IMPORTANT]
>
> **Do not manually change the heater target while Smart Chamber Heater is running.**
>
> The controller intentionally changes the heater target many times during the heating process.
>
> Those changes are part of the control algorithm and are essential for achieving the best heating performance, temperature stability and overshoot reduction.

---

> [!TIP]
>
> Think of the `chamber_heater` temperature as a **request**, not as the actual PTC heater temperature.
>
> You request:
>
> **"I want a 50°C chamber."**
>
> Smart Chamber Heater then decides **how** the heater should operate to reach that temperature in the safest, fastest and most stable way.
