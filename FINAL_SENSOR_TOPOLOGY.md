# Final Sensor Topology — MAP, IAT, and CAN Expansion

**Vehicle:** 2000 US-spec Toyota Celica GT-S / 2ZZ-GE  
**ECU:** legacy ECUMaster EMU Black  
**Status:** SELECTED direction for final custom-harness build; exact sensor hardware not yet selected  
**Checkpoint:** 2026-09-07

## Purpose

Preserve the intended transition from the interim MWR/OEM-harness sensor arrangement to the final Street Build sensor architecture once the EMU Black is mounted in the cabin and the engine/control harness is built from scratch.

This document supplements `BASELINE_PLUS.md`. Baseline Plus is intentionally allowed to retain temporary OEM/MWR sensor paths where they avoid unnecessary current-engine scope.

## 1. Baseline Plus MAP / IAT state

### MAP

Baseline Plus may continue using the EMU Black internal MAP sensor with the existing manifold-pressure hose. Eliminating that hose is **not** a reason to add scope to the interim MWR-adapter commissioning stage.

### IAT

The 2000 2ZZ MAF assembly includes the factory intake-air-temperature sensing element. The current working assumption is that the MWR adapter retains the OEM IAT signal for the EMU while the airflow portion of the MAF is not needed for the intended speed-density strategy.

**Evidence status:** `INFERRED / VERIFY`.

Before relying on that assumption for Baseline Plus, verify the MWR/OEM IAT path by one or more of:

- MWR jumper/PCB continuity;
- EMU input configuration inspection;
- live IAT response while warming/cooling the factory MAF/IAT element.

Do not treat the MAF/IAT assembly as part of the final architecture merely because it remains convenient during Baseline Plus.

## 2. Final Street Build — delete the MAF assembly

When the EMU Black moves into the cabin and the full engine/control harness is built from scratch:

- delete the OEM MAF airflow meter entirely;
- provide a dedicated external MAP sensor near the intake manifold;
- provide a dedicated charge-air IAT sensor downstream of the intercooler;
- keep both MAP and IAT local to the intake/charge system rather than routing long vacuum or sensor assemblies around the engine bay;
- wire primary engine-control sensors directly to the EMU where practical;
- use CAN expansion primarily for secondary instrumentation and additional development sensors.

## 3. External MAP direction

Preferred final arrangement:

```text
charge pipe -> ETB -> manifold interface -> intake manifold
                          |
                          +-> external MAP sensor
                               |
                               +-> very short pressure cavity
                               +-> 5 V / signal / sensor ground
                               +-> direct EMU analog input
```

The EMU internal pressure sensor can then be repurposed for BARO/reference duty if useful to the final calibration.

If an ETB adapter is required between the late-Celica throttle and the 2000 manifold, use that adapter as an integration part and consider machining the MAP boss directly into it.

The Honda-style concept — compact MAP sensor immediately adjacent to the throttle/manifold entry — is the packaging inspiration. **No Honda MAP part number, range, flange, O-ring geometry, or transfer function is selected yet.** Choose the exact sensor first, then design the boss around real dimensional and calibration data.

## 4. Dedicated IAT requirement

The final MAF deletion creates a deliberate IAT requirement.

Preferred measurement location:

- **after the intercooler**;
- **before or immediately downstream of the throttle body**;
- as close as practical to the air actually entering the manifold;
- positioned to minimize wall/metal heat-soak bias while maintaining good exposure to the charge stream.

The preferred sensor should be a fast-response open-element or similarly low-thermal-mass automotive IAT sensor appropriate for boosted operation.

Do not place the final IAT upstream of the intercooler merely because the factory MAF/IAT was located in the inlet tract. The ECU needs charge temperature representative of what the engine actually receives.

## 5. ETB-adapter integration opportunity

If an ETB adapter is required, treat it as a possible **sensor integration block** rather than only a bolt-pattern converter.

Two valid directions remain open:

### Separate sensors

```text
ETB adapter
  ├-> dedicated MAP sensor
  └-> nearby dedicated fast-response IAT sensor in charge pipe/manifold entry
```

Advantages:

- each sensor can be optimized independently;
- easy replacement with common motorsport/OEM parts;
- IAT probe can be located for best airflow exposure rather than constrained by MAP packaging.

### Combined TMAP

```text
ETB adapter / manifold entry
        |
        +-> combined pressure + temperature sensor
```

A suitable automotive TMAP sensor could combine MAP and IAT at one local interface and reduce connector/wire count. This is an **investigation**, not a selected part strategy. Confirm pressure range, temperature response, packaging, accuracy, connector ecosystem, and EMU calibration support before selecting one.

## 6. Final I/O hierarchy

Preferred signal hierarchy for the finished harness:

### Direct to EMU — primary control / protection

- crank;
- cam;
- knock;
- DBW motor;
- TPS/PPS;
- Bosch LSU wideband;
- external MAP;
- dedicated IAT;
- flex-fuel content;
- fuel pressure where practical;
- oil pressure where practical.

### CAN expansion — secondary / development instrumentation

Candidates include:

- A/C pressure if an analog input must be freed;
- EMAP;
- multi-channel EGT module;
- oil temperature;
- coolant pressure;
- fuel temperature;
- gearbox temperature;
- additional pressure/temperature channels;
- temporary development sensors.

Do not move a direct, proven primary-control sensor onto CAN merely to make the topology look uniform. CAN expansion should add capability without unnecessarily increasing dependency for core engine operation.

## 7. Final-build verification gates

Before final harness I/O freeze:

1. Select exact external MAP sensor and pressure range.
2. Select exact IAT sensor or TMAP strategy.
3. Validate intended MAP/IAT mounting geometry against the final ETB/manifold/charge-pipe architecture.
4. Decide which Baseline Plus analog channels remain direct and which lower-priority channel, if any, migrates to CAN to free the external MAP/IAT allocation.
5. Confirm EMU calibration data and fault-detection behavior for every selected sensor.
6. Preserve connector, terminal, seal, wire, and pinout information in the final harness documentation.
