# Final Sensor Topology — MAP, IAT, and CAN Expansion

**Vehicle:** 2000 US-spec Toyota Celica GT-S / 2ZZ-GE  
**ECU:** legacy ECUMaster EMU Black  
**Status:** MAP hardware SELECTED; final IAT hardware and final-harness I/O still open  
**Checkpoint:** 2026-09-16

## Purpose

Preserve the intended transition from the current PowerFC / FC-Datalogit development arrangement through Baseline Plus and into the final Street Build sensor architecture once the EMU Black is mounted in the cabin and the engine/control harness is built from scratch.

This document supplements `BASELINE_PLUS.md`. Baseline Plus is intentionally allowed to retain temporary OEM/MWR paths where they avoid unnecessary current-engine scope, but the MAP sensor itself is now intended to carry through all phases.

## 1. Selected MAP sensor — cross-phase standard

The selected manifold-pressure sensor is:

- **AEM 30-2130-50** stainless pressure sensor;
- **50 PSIa / nominal 3.5 bar absolute** range;
- **0 to 343.385 kPa absolute** calibrated pressure range;
- **0.5 to 4.5 V** calibrated analog output;
- 5 V supply;
- less than 6 mA supply current;
- less than 1 ms response time;
- 1/8 NPT male process connection;
- Packard/GM-style 3-pin electrical connection;
- purchased **2026-09-16** from Ballenger Motorsports.

Nominal transfer used for engineering/logging work:

`MAP [kPa absolute] = (Voltage [V] - 0.5) × 85.84625`

Equivalent anchor points:

- 0.5 V -> 0 kPa absolute;
- 4.5 V -> 343.385 kPa absolute / 50 PSIa.

Do not extrapolate beyond the calibrated 0.5–4.5 V range for control or protection decisions.

### Range decision

The 3.5-bar sensor is accepted for the current Street Build envelope.

At nominal sea-level atmosphere, 343.385 kPa absolute corresponds to approximately **35.3 psi gauge boost**. That is sufficient for the present roughly 500-whp street-build design envelope while retaining materially better resolution than selecting a substantially larger pressure range without a demonstrated need.

For control/protection use, do not design normal operation against the sensor ceiling. Revisit the range if future tuning requires normal manifold pressure above approximately **300 kPa absolute** (about 29 psi gauge at sea level), because the remaining headroom is useful for overboost detection and fault discrimination.

If that threshold is genuinely exceeded, the natural same-family fallback is the **AEM 30-2130-75 75-PSIa / nominal 5-bar absolute** sensor. Do not upsize pre-emptively merely for theoretical future headroom.

## 2. Current PowerFC / CeliTune implementation

The AEM sensor is first being installed as an independent physical-MAP reference while the car remains on the PowerFC.

Preferred temporary architecture:

```text
PowerFC / OEM cable-throttle TPS branch
                |
                +-> removable TPS T-harness
                      |
                      +-> VC / regulated 5 V -> AEM MAP +5 V
                      +-> E2 / sensor ground -> AEM MAP ground
                                              +-> Datalogit ground-reference lead

AEM MAP signal -------------------------------> FC-Datalogit auxiliary analog input
```

The factory harness is not cut. A Ballenger **CONN-86036** TPS extension is used as the sacrificial/removable T-harness base. The 2000 Celica TPS connector is Toyota **90980-11261**; the extension preserves the OEM TPS path while providing local access to VC and E2.

The AEM sensor signal and a sensor-ground reference are routed back to the FC-Datalogit auxiliary interface. **Verify the exact Datalogit auxiliary-channel pair, connector pinout, voltage limits, and differential/single-ended configuration before final termination.** Do not treat the current proposed AN assignment as production wiring until that check is complete.

CeliTune should retain both raw auxiliary voltage and the scaled physical `MAP_kPa_abs` value where available. The channel is not promoted to authoritative physical MAP until KOEO/barometric, vacuum, and positive-pressure plausibility checks have been completed.

## 3. Baseline Plus MAP / IAT state

### MAP

The **AEM 30-2130-50 remains the selected MAP hardware** when the car transitions from PowerFC/Datalogit to EMU Black.

Baseline Plus may temporarily use the EMU Black internal MAP sensor if the interim MWR-adapter I/O allocation cannot accept the external sensor without displacing a higher-priority DBW/protection input. That is an interim I/O accommodation, not a different MAP-sensor selection.

Before the AEM sensor becomes the EMU's primary control MAP:

- assign and document a suitable direct analog input;
- power it from the EMU 5 V sensor reference;
- return it to EMU sensor ground, not chassis ground;
- enter the verified transfer function;
- perform KOEO/barometric and pressure-reference sanity checks;
- confirm no clipping under expected boost and overboost-test conditions.

The EMU internal pressure sensor may then be retained as a secondary BARO/reference channel if useful.

### IAT

The 2000 2ZZ MAF assembly includes the factory intake-air-temperature sensing element. The current working assumption is that the MWR adapter retains the OEM IAT signal for the EMU while the airflow portion of the MAF is not needed for the intended speed-density strategy.

**Evidence status:** `INFERRED / VERIFY`.

Before relying on that assumption for Baseline Plus, verify the MWR/OEM IAT path by one or more of:

- MWR jumper/PCB continuity;
- EMU input configuration inspection;
- live IAT response while warming/cooling the factory MAF/IAT element.

Do not treat the MAF/IAT assembly as part of the final architecture merely because it remains convenient during Baseline Plus.

## 4. Final Street Build — delete the MAF assembly

When the EMU Black moves into the cabin and the full engine/control harness is built from scratch:

- delete the OEM MAF airflow meter entirely;
- retain the **AEM 30-2130-50** as the primary manifold absolute-pressure sensor unless the verified boost envelope forces the 5-bar range decision above;
- provide a dedicated charge-air IAT sensor downstream of the intercooler;
- keep MAP and IAT local to the intake/charge system rather than routing long vacuum or sensor assemblies around the engine bay;
- wire primary engine-control sensors directly to the EMU where practical;
- use CAN expansion primarily for secondary instrumentation and additional development sensors.

## 5. Final MAP packaging direction

Preferred final arrangement:

```text
charge pipe -> ETB -> manifold interface -> intake manifold
                          |
                          +-> AEM 30-2130-50 MAP
                               |
                               +-> very short pressure cavity / hose
                               +-> 5 V / signal / sensor ground
                               +-> direct EMU analog input
```

The sensor has a 1/8 NPT male process connection and is supplied with pressure-interface hardware. Final mounting should be based on actual ETB/manifold geometry and should minimize pressure-line volume, heat exposure, vibration, and service difficulty.

If an ETB adapter is required between the late-Celica throttle and the 2000 manifold, use that adapter as an integration part and consider machining a suitable MAP pressure takeoff/boss into it. The final geometry should be designed around the actual AEM sensor and its fittings rather than around an unselected Honda-style sensor footprint.

## 6. Dedicated IAT requirement

The final MAF deletion creates a deliberate IAT requirement.

Preferred measurement location:

- **after the intercooler**;
- **before or immediately downstream of the throttle body**;
- as close as practical to the air actually entering the manifold;
- positioned to minimize wall/metal heat-soak bias while maintaining good exposure to the charge stream.

The preferred sensor should be a fast-response open-element or similarly low-thermal-mass automotive IAT sensor appropriate for boosted operation.

Because MAP hardware is now selected separately, a combined TMAP is no longer the preferred direction unless a future requirement provides a compelling reason to duplicate/replace the AEM pressure channel.

Do not place the final IAT upstream of the intercooler merely because the factory MAF/IAT was located in the inlet tract. The ECU needs charge temperature representative of what the engine actually receives.

## 7. Final I/O hierarchy

Preferred signal hierarchy for the finished harness:

### Direct to EMU — primary control / protection

- crank;
- cam;
- knock;
- DBW motor;
- TPS/PPS;
- Bosch LSU wideband;
- **AEM 30-2130-50 external MAP**;
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

## 8. Connector / harness state

Current MAP-related purchased hardware:

- **AEM 30-2130-50** MAP sensor kit — purchased 2026-09-16;
- sensor kit includes a mating connector/pin kit;
- **Ballenger CONN-75963** 3-way GM/AEM pressure-sensor receptacle kit — purchased as an additional/spare connector set;
- **Ballenger CONN-86036** 3-way TPS extension — purchased as the removable PowerFC-era T-harness base;
- 20 AWG TXL wire and open-barrel splice terminals purchased for development harness construction.

Production-harness approval of connector housings, terminals, seals, wire colors/gauges, strain relief, and branch routing remains governed by `HARNESS_CONNECTORS.md`.

## 9. Final-build verification gates

Before final harness I/O freeze:

1. **MAP sensor hardware is selected:** AEM 30-2130-50; reopen only if the verified pressure envelope requires a larger range.
2. Verify the AEM transfer against KOEO barometric pressure and at least one trusted vacuum/positive-pressure reference before using it for protection/control.
3. Select exact final IAT hardware.
4. Validate intended MAP/IAT mounting geometry against the final ETB/manifold/charge-pipe architecture.
5. Resolve the Baseline Plus and final direct analog-input allocation for the external MAP without displacing higher-priority protection/DBW channels.
6. Confirm EMU calibration data and fault-detection behavior for every selected sensor.
7. Preserve connector, terminal, seal, wire, and pinout information in the final harness documentation.

## Manufacturer references

- AEM 30-2130-50 product page: https://www.aemelectronics.com/products/sensors/map_sensor/parts/30-2130-50
- AEM 30-2130-50 sensor data: https://documents.aemelectronics.com/techlibrary_30-2130-50_sensor_data.pdf
- AEM 30-2130-XXX pressure-sensor instructions: https://documents.aemelectronics.com/aed016c791759036e1b92adbbb7ef6d3286dd279.pdf
