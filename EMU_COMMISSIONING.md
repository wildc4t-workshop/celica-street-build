# EMU Black Commissioning and 2ZZ Calibration Record

## 1. Purpose and scope

This document is the durable controls/ECU working record for the Celica Street Build. It preserves:

- the actual EMU Black hardware/software baseline;
- the imported 2ZZ reference calibration and V2 -> V3 migration debt;
- bench trigger/synchronization validation;
- settings that imported successfully but are not yet validated for this car;
- commissioning gates that must be closed before the relevant engine functions are enabled.

The imported 2ZZ project is a **reference/start point, not a validated Celica calibration**. Successful import means that data transferred into V3; it does not prove that the resulting setting is correct for the 2000 GT-S, the interim MWR adapter configuration, or the final custom-harness/built-engine configuration.

Original migration evidence is preserved at [`references/emu/2zz_v2-to-v3_import_log.txt`](references/emu/2zz_v2-to-v3_import_log.txt).

## 2. ECU hardware/software baseline

Status: **BENCH-TESTED** as of 2026-09-03.

| Item | Established state |
|---|---|
| ECU | ECUMaster EMU Black |
| Hardware generation | Legacy Micro-USB, black 39-pin + gray 24-pin connector architecture |
| Hardware revision | F |
| CPU revision | G |
| Bootloader | 2.004 |
| Firmware | 3.061 |
| Client | 3.061 |
| Lock state | Unlocked |
| Bench power | Successfully powered from current-limited 13.5 V bench supply |
| Communications | Successfully connected to EMU Black client over Micro-USB |
| Interim vehicle interface | MWR adapter/jumper harness available |

Do not assume newer USB-C-generation pinout documentation applies to this unit without checking the legacy hardware documentation.

## 3. 2ZZ reference-project import

A 2ZZ DBW reference project using definition file **2.174** was imported into EMU Black V3.061. The importer reported that both the definition file and project loaded successfully.

Useful imported reference information includes:

- 4 cylinders;
- coil-on-plug ignition with built-in amplifiers;
- firing order imported as **1-3-4-2**;
- CAM #1 and CAM #2 sensor/trigger settings imported;
- VVT/VVL-related settings imported;
- DBW characteristic tables imported.

None of those imported values are considered verified solely because they imported.

## 4. V2 -> V3 migration warning / verification register

| Area | Import warning / migration concern | Required action | Commissioning gate | Status |
|---|---|---|---|---|
| TPS / PPS | TPS and PPS configuration imported; importer requires input/check-function setup | Verify channel assignment, ranges, polarity/plausibility and calibration against the actual pedal/throttle hardware | Before DBW enable | OPEN |
| Gear detection | Import set number of gears to 7 | Set correct gear count and verify ratios / VSS strategy | Before road validation | OPEN |
| Cranking injection | V3 no longer offers the same batch-all-injectors cranking option | Review V3 cranking strategy rather than reproducing V2 behavior literally | Before first fire | OPEN |
| Cranking airflow | Cranking-airflow table requires adjustment for throttle opening during crank | Configure once actual throttle/DBW strategy is known | Before first fire | OPEN |
| Cranking fueling | V3 cranking tables are VE-based rather than direct injector-time tables | Rebuild/review cranking fuel in the V3 framework | Before first fire | OPEN |
| ASE | After-start enrichment strategy changed | Review and tune ASE 1 / ASE 2 | Startup tuning | OPEN |
| Overrun | Overrun is a separate V3 strategy and imported behavior may differ | Review fuel-cut / decel / overrun behavior | Before street validation | OPEN |
| Acceleration enrichment | Strategy changed and importer states it must be set up manually | Configure and tune in V3 | Before drivability validation | OPEN |
| Injector assignment | Import assumes cylinder-to-injector output mapping | Verify physical injector outputs, cylinder numbering, firing order and injection phasing against the actual harness | **Before fuel outputs are enabled** | OPEN |
| Firing order | Imported as 1-3-4-2 but explicitly flagged for verification | Verify against 2ZZ documentation and actual output assignment | Before spark/fuel enable | OPEN |
| Ignition assignment | Import assumes cylinder-to-ignition-output mapping | Verify physical coil outputs against the actual harness | **Before ignition outputs are enabled** | OPEN |
| Knock | Importer requires sensor-to-cylinder assignment verification | Configure actual knock sensor strategy and validate cylinder relationship before relying on knock protection | Before knock protection is trusted | OPEN |
| Idle ignition | V3 default idle-ignition settings were substituted | Configure idle ignition PID / targets | Idle commissioning | OPEN |
| Idle airflow | Idle-airflow PID requires setup | Configure and tune | Idle commissioning | OPEN |
| DBW | DBW characteristics imported but wizard is still required | Run DBW wizard; verify APP/TPS redundancy, direction, limits and failsafe behavior | **Before DBW is enabled** | OPEN |
| VVT | Importer requires VVTi CAM1 offset verification | Establish true cam phase/reference and verify commanded vs measured angle | Before closed-loop VVT | OPEN |

## 5. Imported does not mean validated

The following areas imported or copied without a migration warning but still require hardware-appropriate review before the relevant commissioning stage:

| Area | Verification required |
|---|---|
| CLT | Confirm selected sensor and resistance/temperature calibration |
| IAT | Confirm selected sensor and calibration |
| Oxygen / lambda | Confirm sensor/controller architecture, calibration and expected lambda source |
| MAP / BARO | Confirm pressure source, range, calibration and whether internal/external reference is used |
| Oil pressure | Confirm whether the imported configuration corresponds to the actual sensor/interface |
| Fuel level / steering / gear ratios / VSS | Verify only if those functions are retained or used |
| Injector calibration | Replace/confirm for actual injector part number, fuel pressure and fuel type |
| Dwell | Verify against actual coil hardware |
| Ignition tables | Treat as reference only; final timing requires real engine/tuner validation |
| VE / fueling tables | Treat as reference/startup material only |
| Lambda targets | Review for the actual turbo/fuel/boost operating envelope |
| Coolant fan settings | Verify fan relay ownership, temperature thresholds and vehicle wiring |
| Fuel-pump settings | Verify relay/output ownership and prime/run behavior |
| Main-relay settings | Verify against interim and final power architecture |
| VVT tables | Verify against actual 2ZZ cam response and final engine configuration |
| VVL / "VTEC" strategy | Verify output ownership, thresholds and oil-pressure/lift strategy |

## 6. Bench-validation plan

Bench work is intentionally narrow. The immediate goal is to make the real EMU Black believe the actual 2ZZ trigger system is rotating before adding CAN, DBW, dyno instrumentation or other simulated vehicle functions.

### Step 1 — 2ZZ crank/cam synchronization

**Goal:** Arduino Uno + Ardu-Stim reproduce the actual 2ZZ crank/cam strategy, and the EMU reports stable synchronization and commanded RPM.

Reference target:

- crank / primary trigger: 2ZZ 36-position wheel with 2 missing teeth (36-2);
- cam sync: actual 2ZZ cam pattern and phase relationship;
- EMU legacy inputs: **B8 Primary Trigger**, **B21 Cam Sync IN #1**;
- Ardu-Stim source: Arduino Uno R3;
- RPM control: software-commanded RPM first; optional A0 potentiometer afterward.

Do not declare the custom Ardu-Stim pattern verified until crank and cam waveforms/phasing have been checked against the chosen authoritative 2ZZ/EMU references.

### Step-1 evidence table

| Test | Acceptance criterion | Status |
|---|---|---|
| EMU bench power | Stable client connection at current-limited bench power | **BENCH-TESTED** |
| EMU USB communication | Client connects and reports ECU state | **BENCH-TESTED** |
| Arduino crank waveform | Expected 2ZZ crank pattern visible on oscilloscope | PENDING |
| Arduino cam waveform | Expected 2ZZ cam pattern visible on oscilloscope | PENDING |
| Crank/cam phase | Pattern relationship agrees with selected 2ZZ reference | PENDING |
| EMU crank synchronization | Synchronized at cranking RPM without trigger errors | PENDING |
| EMU cam synchronization | Stable phase/cam sync | PENDING |
| RPM tracking | EMU follows commanded RPM from cranking through useful test range | PENDING |
| RPM-pot control | Optional physical dial changes commanded RPM smoothly | OPTIONAL |

### Stop condition for Step 1

Step 1 is complete when:

1. the Arduino output is scoped and confirmed;
2. EMU reports stable crank/cam synchronization;
3. commanded RPM is tracked correctly through the chosen test range;
4. trigger errors remain absent / understood;
5. the actual configuration and evidence are recorded here.

Do **not** add dyno-box development, DBW, CAN, output LEDs or a full ECU simulator panel to this task merely because they are interesting.

## 7. Real-engine commissioning gates

The imported project should mature through explicit gates rather than one large "base map verified" state.

### Gate A — trigger / phase

- crank and cam pattern verified;
- sensor type/polarity/thresholds verified on the real engine;
- stable crank and cam synchronization;
- TDC/reference angle verified mechanically;
- VVT home/offset verified.

### Gate B — physical output ownership

Before enabling fuel or spark:

- firing order verified;
- injector output-to-cylinder assignment verified;
- ignition output-to-cylinder assignment verified;
- coil type/dwell verified;
- injector calibration verified.

### Gate C — sensor / DBW sanity

- CLT, IAT, MAP/BARO and lambda sanity checked;
- actual TPS/PPS channels validated;
- DBW wizard completed if DBW is active;
- redundant pedal/throttle plausibility and failsafes verified;
- required pressure/protection channels calibrated before protection logic relies on them.

### Gate D — first fire / idle

- V3 cranking fuel reviewed;
- cranking airflow reviewed;
- initial VE / lambda strategy appropriate for hardware;
- ASE reviewed;
- idle airflow / idle ignition strategy configured sufficiently for commissioning.

### Gate E — running-engine controls

- VVT offset and control validated;
- VVL control validated;
- cooling fan, fuel pump and main relay behavior validated;
- knock strategy validated before relying on it;
- acceleration enrichment / overrun / drivability strategies reviewed.

### Gate F — boost / protection / final calibration

- final boost-control architecture configured;
- protection sensors and thresholds validated;
- tuner reviews protection strategy;
- high-load calibration proceeds only after preceding gates are closed.

## 8. Configuration/evidence archive

Preserve future evidence here or under `references/emu/` as appropriate:

- raw V2 -> V3 import logs;
- screenshots/exported values for trigger configuration;
- Ardu-Stim 2ZZ wheel definition/version;
- oscilloscope captures of simulated crank/cam;
- EMU trigger-scope captures;
- bench-test notes and commanded-vs-reported RPM results;
- tuner-reviewed configuration snapshots where useful.

Do not rely on chat history, ECU project files alone, or tuner memory as the only record of why a commissioning setting is considered valid.
