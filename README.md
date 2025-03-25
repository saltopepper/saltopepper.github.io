# TimeManager Plugin for Unity

This Unity plugin provides a solution for managing time scale-independent behavior in games, enabling smooth transitions between time scales (including `timeScale = 0`) and ensuring specific game objects remain unaffected by global time scale changes. It consists of two core components: `TimeManager` and `TimeExcluder`, written in C# for Unity.
For a better overview you can look at the documentation at https://saltopepper.gitbook.io/timemanager-documentation

## Overview

The **TimeManager Plugin** enhances Unity's time management by:
- Allowing smooth lerping between time scales.
- Supporting limited physics simulations at `timeScale = 0`.
- Excluding specific game objects (e.g., rigidbodies, animations, particle systems) from time scale effects using customizable preservation methods (momentum or energy).

This plugin is particularly useful for games requiring control over time manipulation, such as slow-motion effects, pause mechanics, or physics-driven simulations.

## Features

### TimeManager
- **Smooth Time Scale Transitions**: Lerp between time scales with adjustable duration.
- **Zero Time Scale Support**: Simulates physics manually when `timeScale = 0`.
- **Event System**: Notifies subscribers of time scale changes via `OnTimeChange`.
- **Gravity Toggle**: Optional gravity application at `timeScale = 0` via `ExperimentGravity`.

### TimeExcluder
- **Modular Time Exclusion**: Adjusts game objects (rigidbodies, animations, animators, particle systems) to operate independently of `Time.timeScale`.
- **Preservation Options**: Choose between **momentum preservation** (`p = m * v`) or **energy preservation** (`E = 1/2 * m * v²`) for rigidbody adjustments.
- **Collision Handling**: Supports perfect bounces at `timeScale = 0`.
- **Accurate Collision Mode**: Optionally switches to continuous dynamic collision detection for precision.

## Installation

1. **Clone or Download**: Obtain the plugin as a TimeManager.unitypackage
2. **Add to Unity Project**: Just open the package, while unity is open or drag'n'drop the package into your unity scene.

## Usage

### Setup
1. **TimeManager**:
   - Attach the `TimeManager` script to a GameObject or use the provided Prefab.
   - (Optional) Assign GameObjects to the `m_adjustedObjects` list in the Inspector to automatically add `TimeExcluder` components on start.

2. **TimeExcluder**:
   - Attach to GameObjects with components like `Rigidbody`, `Animation`, `Animator`, or `ParticleSystem`.

### Example Code
```csharp
using TimeManagerPlugin;

.
.
.

// Adjust time scale instantly
TimeManager.AdjustTimeScale(0.5f);

// Smoothly lerp to a new time scale over 2 seconds
TimeManager.Instance.LerpTimeScale(0f, 2f);

// Gradually move towards a target time scale
TimeManager.MoveTowardsTimeScale(1f, 0.1f);
```

### In-Game Behavior
- **Rigidbody**: Velocity, mass, drag, and bounciness are adjusted based on the chosen preservation method.
- **Animation**: Manually sampled to run on unscaled time.
- **Animator & Particle System**: Use built-in unscaled time modes when `timeScale ≠ 1`.

## Configuration

### TimeManager
- `ExperimentGravity`: Enable/disable gravity simulation at `timeScale = 0`.

### TimeExcluder
- `m_preserveMomentum`: Select `Momentum` or `Energy` for rigidbody adjustments.
- `AccurateCollisionMode`: Enable for continuous collision detection at non-standard time scales.

## Notes
- Ensure `TimeManager` is added to the scene before using its static methods.
- The plugin assumes a standard Unity physics setup;
- Tested with Unity Version 2022.3.42f1

## License
This plugin is provided under the [MIT License](LICENSE). Feel free to use, modify, and distribute it as needed.

## Acknowledgments
Developed as part of a Bachelorarbeit project by Thanh Dang Duc exploring time manipulation in game development. Special thanks to Unity's awesome engine and all my colleagues and mentors.
