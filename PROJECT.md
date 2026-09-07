# Celica Street Build — Project State

**Vehicle:** 2000 US-spec Toyota Celica GT-S  
**Role:** finished street car and engineering exercise platform  
**Checkpoint:** 2026-09-07

## 1. Objective

The Street Build is intended to be the **last major drivetrain rebuild of the Celica**.

Target roughly **500 whp maximum capability**, but normal operation should be conservative, predictable, serviceable, and pleasant enough for ordinary street use. Future evolution should mainly be calibration, suspension/tires, NVH/interior refinement, cosmetics, and modular turbo changes rather than another fundamental drivetrain redesign.

## 2. Governing principles

- Keep the current car usable as long as practical.
- Use the spare subframe/replacement drivetrain as the integration buck.
- Build hard-to-revisit interfaces once: drivetrain, controls, harnessing, fueling, turbo interfaces, and service access.
- Prefer mature OEM/off-the-shelf solutions where they satisfy the requirement.
- Optional experiments need an exit ramp and may not hold the car hostage.
- Preserve factory body/cluster functionality and A/C where practical.

## 3. Program sequence

### Stage A — Baseline

Owned by the separate `celica-baseline` project:

- maintenance caught up;
- A/C sorted;
- hydraulic power steering sorted;
- seats installed.

Result: a sorted current-powertrain street car.

### Stage B — Baseline Plus

Use the running current engine to validate the systems worth carrying forward:

1. Preserve PowerFC calibration, datalogs, MPX/body behavior, fan/A/C behavior, and cluster dependencies.
2. Install the MWR return fuel system.
3. Install EMU Black through the MWR adapter.
4. Pull late-2ZZ OEM-style DBW forward if the purchased ETB fits and passes bench validation.
5. Add flex fuel, native LSU 4.9 lambda, fuel pressure, oil pressure, and EMU-native boost control.
6. Retain the MWR PCB as the interim body/cluster integration bridge while modifying only the removable jumper/sub-harness.
7. Commission/tune the package and validate cold/hot behavior, DBW, VVT/VVL, protection, boost control, diagnostics, and retained vehicle functions.
8. Upgrade tires before exploiting materially more power.

The authoritative Stage-B hardware/I/O/buy-list/sub-harness record is [`BASELINE_PLUS.md`](BASELINE_PLUS.md).

### Stage C — Replacement drivetrain build

Develop the major package offline around:

- built 2ZZ;
- E153 factory-LSD transmission;
- MWR clutch/flywheel, mounts/adapters, and axles;
- complete spare Celica subframe and rack;
- final turbo/intake/charge architecture once selected;
- EMU Black;
- DBW;
- flex fuel;
- purpose-built engine/control harness.

Resolve packaging, service access, sensor mounting, and major control interfaces off-car wherever practical.

### Stage D — Final swap / commissioning

Desired sequence:

> Disconnect chassis interfaces -> remove current drivetrain/subframe -> install substantially complete replacement module -> reconnect known supporting interfaces -> fluids -> startup/commissioning.

Do not duplicate ordinary chassis-side hardware solely to make the floor assembly look complete.

## 4. Selected final-build requirements

- 2ZZ remains the engine architecture.
- Turbocharged street build remains the direction.
- EMU Black replaces the Apexi PowerFC.
- DBW is required.
- Flex fuel is required.
- A full custom engine/control harness is the intended final architecture.
- EMU Black moves into the cabin for the final harness.
- Final build deletes the OEM MAF assembly.
- Final build uses a local external MAP sensor and dedicated post-intercooler IAT/TMAP strategy.
- CAN expansion is preferred for secondary/development instrumentation after direct I/O is consumed.
- A/C remains functional.
- Practical factory body/cluster behavior should be preserved.
- Approx. 500 whp is a design envelope, not the default driving mode.

The final MAP/IAT/CAN direction is owned by [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md).

## 5. Controls strategy

### Baseline Plus

- legacy EMU Black, hardware rev F / CPU rev G, firmware/client 3.061;
- MWR adapter retained as interim Celica integration layer;
- removable sub-harness handles DBW/VVT/pressure/flex/lambda/boost additions;
- H-Bridge 1A/1B -> ETB motor;
- AUX6 -> VVT after proper OCV power/low-side conversion;
- H-Bridge 2A -> VVL;
- Injector 6 / G22 -> existing Tru-Boost MAC valve for EMU-native PWM boost control;
- Link MIPS 101-0325 sensors selected for fuel and oil pressure;
- Bosch LSU 4.9 selected for native lambda;
- GM/Continental 13507129 + Radium 20-0589 selected for flex fuel.

Detailed I/O and verification gates live in [`BASELINE_PLUS.md`](BASELINE_PLUS.md). ECU migration/bench/commissioning evidence lives in [`EMU_COMMISSIONING.md`](EMU_COMMISSIONING.md).

### Final

- remove MWR adapter architecture;
- wire EMU directly through the custom harness;
- carry forward proven DBW/flex/lambda/pressure/boost-control strategies where practical;
- use local external MAP + dedicated IAT/TMAP instead of long vacuum plumbing / factory MAF;
- expand over CAN for EMAP, multi-channel EGT, oil temperature, coolant pressure, and similar secondary/development channels as justified.

## 6. Known replacement-drivetrain hardware

### Engine

Confirmed/recalled hardware:

- sleeved 2ZZ block;
- forged low-compression pistons;
- upgraded rods;
- ARP main hardware, head studs, and rod bolts;
- OEM MLS head gasket;
- stock cams;
- upgraded valve springs and titanium retainers;
- new oil pump, timing chain, lift bolts, thermostat, and water pump;
- Moroso upgraded oil pan;
- upgraded harmonic balancer currently being installed.

Still verify from build records:

- compression ratio, remembered around 9.5:1;
- upgraded stainless valve specification;
- balancer make/model, believed ATI;
- valve clearance before final installation.

### Transmission / chassis integration

- E153 with factory LSD;
- MWR clutch/flywheel;
- MWR mounts/adapters;
- MWR axles;
- complete spare Celica subframe;
- power-steering rack already on spare subframe;
- Mishimoto radiator already on current car.

## 7. Fuel / protection hardware selected for Baseline Plus

Owned:

- MWR return-style system;
- MWR fuel rail;
- AEM FPR;
- AEM fuel pump.

Selected additions:

- Radium 20-0589 split-flow flex-fuel housing;
- GM/Continental 13507129 ethanol sensor;
- 2 × Link MIPS 101-0325 150-PSI pressure sensors;
- Bosch LSU 4.9;
- MWR MWR-901465 oil-filter sandwich plate;
- remote -3AN oil-pressure sender plumbing;
- existing Tru-Boost MAC valve under EMU control.

Exact buy state and unresolved fittings are tracked in `BASELINE_PLUS.md` / `tasks.csv`, not duplicated here.

## 8. Turbo / hot-side architecture

### Known viable fallback

The existing TurboKits.com T28-flanged architecture remains usable and is the low-risk fallback.

### Modular sidewinder investigation

Still under investigation:

- driver-side / sidewinder turbo placement;
- compact stainless collector/manifold;
- first-priority external-wastegate takeoff;
- V-band collector termination;
- replaceable turbo-specific up-pipe;
- future modular turbo changes without redesigning the engine-side collector.

The cheap cast TD05-style manifold and inexpensive 20G are development/test hardware, not selected final parts.

## 9. Intake / charge / DBW

- 2003–2005 Celica GT-S OEM ETB purchased 2026-09-07 for fitment/POC.
- 2003–2005 Celica accelerator pedal owned.
- If the late ETB needs an adapter, the adapter may also integrate the final external MAP boss.
- Corolla 2ZZ runners remain available development hardware.
- Existing TurboKits.com intercooler architecture remains a viable baseline.
- A2W remains optional; it must earn its complexity.

Exact final intake/plenum/throttle/charge-cooling architecture remains open.

## 10. Harness / connector strategy

The final harness must be rebuildable from documentation without chat history.

First-pass Ballenger connector kits are received for crank/cam, VVT/VVL, oil-pressure-switch family, ECT, late 2ZZ knock, ignition coils, late DBW throttle, pedal, and alternator. Physical-fit and terminal verification remain pending.

Connector verification and final BOM rules are owned by [`HARNESS_CONNECTORS.md`](HARNESS_CONNECTORS.md).

## 11. Pre-EMU evidence requirement

Before PowerFC removal, complete the minimum evidence in [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md):

- body/MPX/cluster behavior;
- fan and A/C behavior;
- native PowerFC map archive;
- controlled cold/hot/cruise/transient/lift/full-load reference datalogs where safe.

Do not delay the EMU transition for open-ended network reverse engineering or an attempted cell-for-cell PowerFC conversion.

## 12. Current open architecture

Remain deliberately open until evidence justifies selection:

- final turbo/hot-side architecture;
- final intake/plenum/throttle-body architecture after ETB POC;
- final charge-cooling architecture;
- exact final external MAP and IAT/TMAP hardware;
- exact final custom-harness topology and I/O allocation;
- CAN expansion hardware and secondary instrumentation set;
- final protection thresholds and display strategy;
- detailed chassis-side transfer list for swap day.

These are real open decisions. Selected Baseline Plus hardware should not be repeatedly reopened unless new evidence creates a reason.
