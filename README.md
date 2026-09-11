# Celica Street Build

Engineering definition, architecture, staging, and execution plan for the final major drivetrain and controls build of a **2000 Toyota Celica GT-S**.

The Street Build is intended to be the **last major drivetrain rebuild** of the car. The finished package should support roughly **500 whp maximum capability** while remaining a civilized, conservative street car in normal use.

## North Star

> Modernize the Celica enough that it does not feel old to live with, while preserving the lightweight analog character that makes it worth keeping. Use proven solutions where they are good enough; engineer custom solutions where they provide real value.

## Program Strategy

1. Finish the narrow current-car mechanical baseline.
2. Preserve the known-good PowerFC/body-function baseline.
3. Use **Baseline Plus** to validate fuel, EMU, DBW, flex fuel, lambda, pressure protection, and boost control on the running current engine.
4. Build the replacement built-2ZZ/E153 package offline on the spare subframe.
5. Build the final custom engine/control harness and final sensor topology around the proven controls strategy.
6. Swap the substantially complete drivetrain package into the car.
7. Finish through calibration, tires/suspension, NVH, interior, and cosmetic refinement rather than another drivetrain redesign.

## Current Handoff — 2026-09-11

The major Baseline A/C / hydraulic-power-steering reassembly is complete and the current car is back in running configuration. The A/C line is supported/protected, hydraulic PS is restored, steering is recentered, the service corridor has been 3D-scanned, turbo coolant/oil-drain service is complete, and the PowerFC A/C idle hunt has been corrected with a localized base-map change.

Baseline remains open for road/maintenance/administrative closeout and the seat path, but it no longer needs to consume the main engineering effort. Street Build bench/design work can proceed in parallel.

Near-term Street Build focus:

- finish the in-progress EMU trigger bench validation;
- preserve a fresh native PowerFC archive from the corrected 2026-09-11 state;
- complete remaining pre-EMU body/electrical/datalog evidence without duplicating Baseline logs;
- physically verify the late-Celica ETB;
- procure and bench the selected Baseline Plus controls/protection package;
- advance built-engine/E153 verification and inventory offline.

## Current Selected Direction

- Turbocharged 2ZZ remains the engine architecture.
- ECUMaster EMU Black replaces the Apexi PowerFC.
- Baseline Plus uses the MWR adapter as an interim Celica/body integration bridge.
- Late-2ZZ OEM-style DBW is being pulled forward into Baseline Plus, subject to ETB fit and bench validation.
- Flex fuel, native LSU 4.9 lambda, fuel pressure, oil pressure, and EMU-native boost control are selected for Baseline Plus.
- The final build uses a purpose-built engine/control harness with the EMU mounted in the cabin.
- The final build deletes the MAF, uses a local external MAP sensor and dedicated post-intercooler IAT, and uses CAN expansion primarily for secondary/development instrumentation.
- A/C and practical factory body/cluster functionality should survive.
- The built 2ZZ, E153, and spare subframe form the replacement drivetrain module.
- The current car stays usable as long as practical.

The exact final turbo/hot-side, intake/plenum, charge-cooling package, final MAP/IAT hardware, CAN expansion hardware, and detailed final protection thresholds remain open until explicitly selected.

## Source of Truth

- [`PROJECT.md`](PROJECT.md) — durable program state, staging, hardware, and open architecture decisions.
- [`tasks.csv`](tasks.csv) — executable work queue and status.
- [`project.yaml`](project.yaml) — compact machine-readable project state.
- [`AGENTS.md`](AGENTS.md) — engineering-record and collaboration rules.
- [`BASELINE_PLUS.md`](BASELINE_PLUS.md) — authoritative interim controls/fuel/DBW/protection architecture, buy list, I/O, and sub-harness.
- [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md) — evidence to capture before PowerFC removal, including the corrected 2026-09-11 PowerFC reference state.
- [`EMU_COMMISSIONING.md`](EMU_COMMISSIONING.md) — EMU hardware/software baseline, reference-map migration, and commissioning gates.
- [`HARNESS_CONNECTORS.md`](HARNESS_CONNECTORS.md) — controlled connector verification and final-harness BOM starting point.
- [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md) — final MAP/IAT/MAF-delete/CAN-expansion direction.
- [`REPLACEMENT_DRIVETRAIN.md`](REPLACEMENT_DRIVETRAIN.md) — replacement-engine/E153 provenance, current separated state, and controlled reassembly record.

Related repositories:

- [`celica-baseline`](https://github.com/wildc4t-workshop/celica-baseline) — current-car mechanical baseline and current drivability closeout.
- [`Celica-engineering-knowledge`](https://github.com/wildc4t-workshop/Celica-engineering-knowledge) — shared factory/reference research.
- [`CeliKey`](https://github.com/wildc4t-workshop/CeliKey) — passive-entry/body-control project.
- [`celica-side-projects`](https://github.com/wildc4t-workshop/celica-side-projects) — BBK and EPS.
- [`celica-project-dashboard`](https://github.com/wildc4t-workshop/celica-project-dashboard) — project dashboard.
