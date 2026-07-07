# Flowcharts

This document contains Mermaid diagrams describing the internal workflow
of Smart Chamber Heater.

------------------------------------------------------------------------

# Overall Control Flow

```mermaid
flowchart TD
    A[Start Control Loop] --> B[Read Sensors]
    B --> C[Read Chamber Target]
    C --> D[Run Safety Checks]
    D -->|Fault| E[Emergency Actions]
    D -->|Safe| F[Select Control Mode]
    F --> G[Compute Heater Target]
    G --> H[Compute Fan Speeds]
    H --> I[Apply Outputs]
    I --> J[Next Control Cycle]
```

------------------------------------------------------------------------

# Automatic Mode

```mermaid
flowchart TD
    A[Calculate Thermal Headroom] --> B{Headroom}
    B -->|Low| C[Linear Control]
    B -->|Medium| D[Hybrid Predictive]
    B -->|High| E[Full Predictive Adaptive Predictive]
```

------------------------------------------------------------------------

# Heating State Machine

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Heating
    Heating --> Hold
    Heating --> Overshoot
    Hold --> Heating
    Hold --> Cooldown
    Overshoot --> Hold
    Cooldown --> Idle
```

------------------------------------------------------------------------

# Prediction Pipeline

```mermaid
flowchart LR
    A[Chamber Temperature]
    B[Heating Rate]
    C[PTC Thermal Memory]
    D[Prediction Horizon]

    A --> E[Prediction Model]
    B --> E
    C --> E
    D --> E

    E --> F[Predicted Chamber Temperature]
    F --> G[Adaptive Heater Target]
```

------------------------------------------------------------------------

# Safety Priority

```mermaid
flowchart TD
    A[Safety Check]
    A --> B{Chamber Overtemperature?}
    B -->|Yes| Z[Shutdown + Fans]
    B -->|No| C{Emergency Heater Limit?}
    C -->|Yes| Z
    C -->|No| D{Soft Limit?}
    D -->|Yes| E[Reduce Heater Target]
    D -->|No| F[Normal Control]
```

------------------------------------------------------------------------

# Hold Mode

```mermaid
flowchart TD
    A[Target Reached]
    A --> B[Hold Enabled?]
    B -->|No| C[Heater Off]
    B -->|Yes| D[Maintain Hold Temperature]
    D --> E[Adaptive Fan]
    E --> F[Stable Chamber]
```

------------------------------------------------------------------------

# Overshoot Recovery

```mermaid
flowchart TD
    A[Overshoot Detected]
    A --> B[Disable Heater]
    B --> C[Adaptive Exhaust]
    C --> D[Circulation Fan Active]
    D --> E{Within Release Window?}
    E -->|No| C
    E -->|Yes| F[Resume Normal Control]
```

------------------------------------------------------------------------

# Cooldown

```mermaid
flowchart TD
    A[Heating Disabled]
    A --> B[PTC Off]
    B --> C[Run Cooling Fans]
    C --> D{Cooldown Temperature Reached?}
    D -->|No| C
    D -->|Yes| E[Stop Fans]
```

------------------------------------------------------------------------

# Notes

These diagrams describe the logical behaviour of the controller.

Individual implementations may differ internally, but the overall
architecture and safety priority remain the same.
