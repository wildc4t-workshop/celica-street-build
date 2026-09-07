# EMU Black Commissioning and 2ZZ Calibration Record

## 1. Purpose

This document owns **ECU commissioning evidence and gates**. It does not own the Baseline Plus hardware BOM or final sensor architecture.

Use:

- [`BASELINE_PLUS.md`](BASELINE_PLUS.md) for the interim hardware/I/O/sub-harness architecture;
- [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md) for PowerFC/body-function capture;
- [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md) for final MAP/IAT/CAN direction.

Reference calibrations are starting material only. Imported settings are not validated merely because they load successfully.

## 2. ECU hardware/software baseline

Status: **BENCH-TESTED** as of 2026-09-03.

| Item | Established state |
|---|---|
| ECU | ECUMaster EMU Black |
| Hardware generation | Legacy Micro-USB; black 39-pin + gray 24-pin connectors |
| Hardware revision | F |
| CPU revision | G |
| Bootloader | 2.004 |
| Firmware / client | 3.061 |
| Lock state | Unlocked |
| Bench power | Stable on current-limited 13.5 V supply |
| USB communication | Verified |
| Interim vehicle interface | MWR adapter + removable jumper harness |

Do not apply newer USB-C-generation pinouts to this unit without verification.

## 3. Reference calibrations — keep their authority separate

Two useful 2ZZ references are available and must not be conflated:

### MWR 2023 supercharged 2ZZ Celica map

**Authority:** MWR/Celica adapter I/O and vehicle-integration reference.

Known useful assignments from the recovered map include:

- Injector 1–4 -> cylinders 1–4;
- Injector 5 -> main relay;
- Injector 6 -> unused in the recovered map;
- AUX1 -> fuel pump;
- AUX2 -> coolant fan;
- AUX3 -> A/C clutch;
- AUX4 -> tach;
- AUX5 -> A/C fan;
- AUX6 -> cable-throttle IAC;
- H-Bridge 1A -> VVT;
- H-Bridge 2A -> VVL;
- CAN 500 kbps with standard EMU stream enabled at base ID `0x600`.

This map is **not** a ready-to-run tune for the user's current turbo engine.

### Lotus 2ZZ DBW reference map

**Authority:** DBW implementation reference, not Celica wiring authority.

Useful concepts:

- H-Bridge 1 used for DBW motor control;
- auxiliary outputs used for VVT/VVL;
- electronic pedal and throttle-position functions configured in EMU software.

A V2 -> V3 migration log is preserved at [`references/emu/2zz_v2-to-v3_import_log.txt`](references/emu/2zz_v2-to-v3_import_log.txt).

## 4. V2 -> V3 migration / validation debt

| Area | Required validation | Gate |
|---|---|---|
| TPS / PPS | Assign actual channels; calibrate main/check tracks; verify plausibility and polarity | Before DBW enable |
| Gear detection | Correct imported gear count / ratios if used | Before road validation |
| Cranking fuel | Rebuild/review in V3 VE framework | Before first fire |
| Cranking airflow | Configure for actual DBW throttle behavior | Before first fire |
| ASE | Review V3 after-start enrichment | Startup tuning |
| Overrun | Review V3 decel/fuel-cut behavior | Before street validation |
| Accel enrichment | Configure manually in V3 | Before drivability validation |
| Injector assignment | Verify output-to-cylinder mapping and actual injector calibration | Before fuel enable |
| Firing order | Verify 1-3-4-2 against hardware assignment | Before spark/fuel enable |
| Ignition assignment | Verify coil output-to-cylinder mapping and dwell | Before ignition enable |
| Knock | Verify sensor strategy and cylinder relationship | Before relying on knock protection |
| Idle ignition / airflow | Configure for DBW idle control | Idle commissioning |
| DBW | Run wizard; verify direction, limits, redundancy, failsafe | Before DBW enable |
| VVT | Establish true cam reference/offset and commanded-vs-measured behavior | Before closed-loop VVT |

## 5. Baseline Plus selected sensor/control architecture

The hardware choice itself is owned by `BASELINE_PLUS.md`; this table records what commissioning must validate.

| Function | Selected Baseline Plus hardware / path | Commissioning requirement |
|---|---|---|
| Lambda | Bosch LSU 4.9 direct to native EMU WBO | Heater/controller status, plausible free-air/running lambda, fault behavior |
| Fuel pressure | Link MIPS 101-0325 on B35 / AIN5 | Confirm 0.5–4.5 V calibration and effective pressure vs MAP |
| Oil pressure | Link MIPS 101-0325 on B37 / AIN6 | Confirm calibration and develop RPM-dependent protection with tuner |
| Flex fuel | GM/Continental 13507129 on B9 | Confirm frequency/ethanol reading against known fuel |
| DBW | late-Celica ETB + pedal | Wizard, redundant-track plausibility, failsafe, idle behavior |
| VVT | G4 / AUX6 low-side after OCV power conversion | Verify wiring, output polarity/frequency, cam response |
| VVL | G3 / H-Bridge 2A | Verify lift output ownership and changeover behavior |
| Boost control | existing Tru-Boost MAC valve on G22 / Injector 6 | Verify electrical suppression, base-boost failsafe, open-loop duty before closed-loop control |
| MAP | EMU internal MAP for Baseline Plus | Confirm hose integrity/range/calibration |
| IAT | expected OEM MAF-integrated IAT path | Verify actual input path and live response before relying on it |

## 6. Bench-validation plan

Immediate bench work stays narrow: prove the actual EMU can synchronize to a credible 2ZZ trigger pattern before adding broader simulator scope.

### Step 1 — crank/cam synchronization

Target:

- 2ZZ crank pattern: 36-position wheel with 2 missing teeth;
- actual 2ZZ cam pattern and phase relationship;
- EMU B8 Primary Trigger;
- EMU B21 Cam Sync IN #1;
- Arduino Uno R3 + Ardu-Stim source;
- scope simulated crank/cam before trusting ECU synchronization.

| Test | Acceptance criterion | Status |
|---|---|---|
| EMU bench power | Stable client connection | **BENCH-TESTED** |
| USB communication | Client reports ECU state | **BENCH-TESTED** |
| Arduino crank waveform | Expected pattern on oscilloscope | PENDING |
| Arduino cam waveform | Expected pattern on oscilloscope | PENDING |
| Crank/cam phase | Agrees with selected 2ZZ reference | PENDING |
| EMU crank sync | Stable at cranking RPM without unexplained trigger errors | PENDING |
| EMU cam sync | Stable phase/cam sync | PENDING |
| RPM tracking | Reported RPM follows commanded RPM through test range | PENDING |

Stop when trigger synchronization is proven and recorded. Do not turn this task into a full ECU simulator panel.

## 7. Real-engine commissioning gates

### Gate A — trigger / phase

- real sensor type/polarity/thresholds verified;
- stable crank/cam synchronization;
- TDC/reference angle verified mechanically;
- VVT home/offset verified.

### Gate B — output ownership

Before fuel/spark enable:

- injector and ignition output-to-cylinder assignments verified;
- firing order verified;
- coil dwell verified;
- injector characterization verified.

### Gate C — sensor / DBW sanity

- CLT, IAT, MAP and lambda plausible;
- Link fuel/oil pressure sensors calibrated;
- flex-fuel reading plausible;
- TPS/PPS main/check channels valid;
- DBW wizard complete and failsafes tested.

### Gate D — first fire / idle

- V3 cranking fuel and airflow reviewed;
- initial VE/lambda strategy appropriate;
- ASE reviewed;
- DBW idle airflow/idle ignition sufficiently configured.

### Gate E — running-engine controls

- VVT offset/control validated;
- VVL control validated;
- cooling fan, fuel pump, main relay, A/C interactions validated;
- knock strategy validated before reliance;
- transient and overrun behavior reviewed.

### Gate F — boost / protection / final calibration

Proceed to meaningful load only after preceding gates close:

- verify MAC plumbing fails to mechanical/base boost with solenoid de-energized;
- establish safe open-loop solenoid duty behavior before closed-loop tuning;
- verify overboost protection;
- verify fuel-pressure-vs-MAP monitoring;
- establish oil-pressure-vs-RPM protection;
- review lambda, knock, CLT/IAT and boost-response protections with tuner;
- then perform high-load calibration.

## 8. Evidence archive

Preserve only evidence that helps reproduce or validate the setup:

- V2 -> V3 migration logs;
- final trigger configuration and scope captures;
- Ardu-Stim wheel definition/version;
- EMU trigger-scope captures;
- commanded-vs-reported RPM results;
- commissioning configuration snapshots;
- tuner-reviewed protection settings where useful.

Do not duplicate the hardware BOM, final sensor roadmap, or task queue here.
