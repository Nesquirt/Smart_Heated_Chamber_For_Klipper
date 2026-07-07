# Frequently Asked Questions (FAQ)

## General

### What is Smart Chamber Heater?

Smart Chamber Heater is an adaptive chamber heating controller for
Klipper designed to improve heat-up speed, chamber stability and safety
by combining predictive algorithms with configurable thermal management.

------------------------------------------------------------------------

### Do I need a heated chamber?

Yes.

The project is intended for enclosed printers using a chamber heater
such as a PTC heater.

------------------------------------------------------------------------

### Does this replace Klipper's heater PID?

No.

The heater PID continues regulating the heater temperature.

Smart Chamber Heater decides **how** the heater should be used.

------------------------------------------------------------------------

## Installation

### Is this compatible with every printer?

The controller is designed to be hardware independent.

Any enclosed printer supported by Klipper can theoretically use it after
including in printer.cfg

------------------------------------------------------------------------

### Can I use multiple chamber fans?

Yes.

The controller supports independent circulation, exhaust and heat-assist
fans.

------------------------------------------------------------------------

### Can I disable Hold Mode?

Yes.

Hold Mode can be completely disabled if desired.

------------------------------------------------------------------------

## Control Algorithms

### Which control mode should I choose?

If unsure:

Use **Automatic Mode**.

------------------------------------------------------------------------

### When should I use Linear Control?

Use Linear Control if your heater struggles to reach the requested
chamber temperature.

------------------------------------------------------------------------

### When should I use Hybrid Predictive Control?

Hybrid Predictive Control is recommended for most printers.

It combines fast heating with predictive unloading.

------------------------------------------------------------------------

### When should I use Full Predictive Adaptive Predictive?

Use Full Adaptive Predictive only when the heater has significant thermal headroom and
overshoot becomes the primary limitation.

------------------------------------------------------------------------

## Prediction

### Why does the controller predict future temperature?

Because the heater still contains thermal energy after its target
changes.

Prediction estimates this remaining energy to reduce overshoot.

------------------------------------------------------------------------

### Is prediction physically accurate?

No.

The controller intentionally uses a simplified thermal model.

The objective is practical control rather than perfect physical
simulation.

------------------------------------------------------------------------

## Hold Mode

### Why keep the heater warm?

Keeping the heater slightly above chamber temperature reduces
temperature oscillation and minimizes heater cycling.

------------------------------------------------------------------------

### Does Hold Mode waste energy?

Not necessarily.

In many systems it actually reduces repeated high-power heating cycles.

------------------------------------------------------------------------

## Safety

### Can I increase the thermal fuse temperature?

No.

Always configure the real thermal fuse installed in your printer.

------------------------------------------------------------------------

### Can I disable safety protections?

Strongly discouraged.

Safety mechanisms are an essential part of the controller.

------------------------------------------------------------------------

### Why does the exhaust fan suddenly start?

The exhaust fan may activate because of:

- Overshoot Cooling
- Emergency Protection
- Automatic Cooldown

------------------------------------------------------------------------

## Tuning

### Which parameter should I tune first?

Always begin with safety parameters, then Hold Mode, followed by
prediction.

------------------------------------------------------------------------

### The chamber heats too slowly.

Possible causes:

- Heater undersized
- Chamber insulation
- Conservative prediction settings

------------------------------------------------------------------------

### The chamber overshoots too much.

Increase:

- Prediction Horizon
- Thermal Weight

or consider Hybrid / Full Adaptive Predictive.

------------------------------------------------------------------------

### The heater unloads too early.

Reduce:

- Prediction Horizon
- Thermal Weight
- Rate Gain

------------------------------------------------------------------------

## Diagnostics

### Which command should I use first?

Start with:

CHAMBER_CONTROL_STATUS

It provides the most complete overview of the controller.

------------------------------------------------------------------------

### How can I inspect prediction?

Use:

CHAMBER_PREDICTION_STATUS

------------------------------------------------------------------------

### How do I verify safety limits?

Use:

CHAMBER_LIMITS

------------------------------------------------------------------------

## Development

### Can I modify the controller?

Yes.

The project is open source.

Contributions are welcome.

------------------------------------------------------------------------

### Can I create custom algorithms?

Yes.

The modular architecture allows additional control strategies to be
implemented while preserving the existing safety layer.

------------------------------------------------------------------------

## Future

### Will more control algorithms be added?

Possibly.

Future versions may include:

- Self-learning prediction
- Adaptive thermal models
- Ambient compensation
- Statistical prediction

while remaining backward compatible whenever possible.
