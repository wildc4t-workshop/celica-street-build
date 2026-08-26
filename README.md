# Celica Street Build

Engineering definition, architecture, staging, and execution plan for the final major drivetrain and controls build of a **2000 Toyota Celica GT-S**.

The goal is not to build a perpetual project car. The Street Build is intended to be the **last major drivetrain rebuild of the Celica**. Once complete, future work should primarily be refinement, tuning, bolt-on evolution, and finish-quality improvements rather than another fundamental drivetrain redesign.

## North Star

> Modernize the Celica enough that it does not feel like an old car to live with, while preserving the lightweight analog character that makes it worth keeping. Use proven solutions where they are good enough; engineer custom solutions where they provide real value.

The finished car should be capable of roughly **500 whp**, but maximum output is a design envelope rather than the default operating condition. Normal use should remain civilized, reliable, and appropriate for ordinary street driving.

## Program Strategy

1. Finish the narrow mechanical baseline on the current car.
2. Preserve the known-good PowerFC/body-function baseline before removing it.
3. Validate independent fuel/EMU upgrades on the running car with minimal downtime.
4. Build the replacement drivetrain package offline on the spare subframe.
5. Swap the substantially complete module into the car.
6. Finish the car through calibration, refinement, suspension/tires, interior, NVH, and cosmetic work rather than another major drivetrain teardown.

## Current Selected Direction

- Turbocharged 2ZZ remains the engine architecture.
- ECUMaster EMU Black replaces the Apexi PowerFC.
- Drive-by-wire is part of the final Street Build.
- Flex-fuel capability is part of the final Street Build.
- A/C survives the build.
- Factory body and cluster functionality should be preserved where practical.
- The built 2ZZ, E153, and spare subframe form the replacement drivetrain module.
- The current car stays intact and usable as long as practical.
- A full custom engine/control harness is the intended final architecture.

The exact turbo, hot-side, intake, charge-cooling, and detailed instrumentation architecture remain open until explicitly selected.

## Source of Truth

- [`PROJECT.md`](PROJECT.md) — durable engineering state, architecture, hardware, staging, and open decisions.
- [`tasks.csv`](tasks.csv) — canonical executable work queue and task status.
- [`project.yaml`](project.yaml) — machine-readable project state for the dashboard.
- [`AGENTS.md`](AGENTS.md) — collaboration and engineering-record rules.
- [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md) — minimum evidence to capture before removing the known-good PowerFC architecture.

Related repositories:

- [`celica-baseline`](https://github.com/wildc4t-workshop/celica-baseline) — current-car mechanical baseline.
- [`Celica-engineering-knowledge`](https://github.com/wildc4t-workshop/Celica-engineering-knowledge) — shared research/reference archive.
- [`CeliKey`](https://github.com/wildc4t-workshop/CeliKey) — standalone passive-entry/body-control project.
- [`celica-side-projects`](https://github.com/wildc4t-workshop/celica-side-projects) — BBK and EPS.
- [`celica-project-dashboard`](https://github.com/wildc4t-workshop/celica-project-dashboard) — public project dashboard.
