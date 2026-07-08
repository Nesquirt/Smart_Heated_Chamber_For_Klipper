# Contributing to Smart Chamber Heater

First of all, thank you for considering contributing to **Smart Chamber
Heater**.

Whether you are reporting a bug, improving the documentation, testing
new hardware or proposing a new control algorithm, every contribution
helps make the project better.

------------------------------------------------------------------------

# Ways to Contribute

You can contribute by:

- Reporting bugs
- Suggesting new features
- Improving the documentation
- Testing on different printers
- Improving control algorithms
- Improving safety mechanisms
- Optimizing performance
- Translating documentation
- Reviewing pull requests

------------------------------------------------------------------------

# Before Opening an Issue

Please check that:

- The issue has not already been reported.
- You are using the latest version.
- The problem can be reproduced consistently.
- Hardware configuration is included.

Useful information includes:

- Printer model
- Klipper version
- Heater type
- Thermal fuse rating
- Chamber target
- Control mode
- Relevant log output

------------------------------------------------------------------------

# Pull Requests

Please keep pull requests focused.

A single pull request should address one topic only.

Examples:

- Documentation update
- Bug fix
- New feature
- Algorithm improvement
- Refactoring

------------------------------------------------------------------------

# Coding Guidelines

When modifying the macro:

- Keep safety logic independent from control logic.
- Never bypass safety protections.
- Prefer readable code over compact code.
- Document every configurable parameter.
- Comment new sections in English.
- Avoid introducing hardware-specific assumptions.

------------------------------------------------------------------------

# Documentation Guidelines

Documentation should:

- Explain *why*, not only *how*.
- Stay synchronized with the macro.
- Use clear technical language.
- Include practical examples whenever possible.

------------------------------------------------------------------------

# Testing

Before submitting changes:

- Test from a cold chamber.
- Verify a complete heating cycle.
- Verify Hold Mode.
- Verify overshoot handling.
- Verify cooldown.
- Verify emergency protection.
- Confirm no regression of existing features.

------------------------------------------------------------------------

# Reporting Bugs

Please include:

- Steps to reproduce
- Expected behaviour
- Actual behaviour
- Printer configuration
- Relevant console output
- Photos or graphs (if useful)

------------------------------------------------------------------------

# Feature Requests

Feature requests are welcome.

Please explain:

- The problem being solved.
- Why the current behaviour is insufficient.
- The proposed solution.
- Possible side effects.

------------------------------------------------------------------------

# Development Philosophy

Smart Chamber Heater follows a few core principles:

- Safety before performance
- Hardware independence
- Predictable behaviour
- Modular architecture
- Clear documentation
- Backward compatibility whenever possible

------------------------------------------------------------------------

# Code of Conduct

By participating in this project you agree to follow the community Code
of Conduct.

Please read `CODE_OF_CONDUCT.md`.

------------------------------------------------------------------------

# Thank You

Every contribution---large or small---is appreciated.

Thank you for helping improve Smart Chamber Heater.
