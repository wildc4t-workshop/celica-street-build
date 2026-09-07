# Pre-EMU Baseline Capture

Before removing the known-good Apexi PowerFC architecture, preserve the vehicle-side behavior and calibration evidence that may be difficult to reconstruct later.

This is an **evidence-capture gate**, not an attempt to fully decode Toyota body networking or directly translate the PowerFC calibration into EMU Black tables before the EMU installation.

## Capture set A — ECU / MPX / cluster electrical baseline

Record on the actual 2000 US-spec GT-S:

- PowerFC MPX1 electrical behavior;
- PowerFC MPX2 electrical behavior;
- KOEO comparison with the PowerFC connected versus disconnected where safe/practical;
- tach waveform at ECM E3-27 and at the combination meter as applicable;
- MIL (`W`) electrical behavior.

Preserve test point, vehicle/ignition state, scope settings, waveform/log filename, and the useful conclusion.

## Capture set B — retained vehicle functions

Record:

- cooling-fan relay truth table;
- A/C request/output behavior with the PowerFC;
- cluster oil-warning behavior and source;
- coolant-gauge sender topology on the actual car.

The purpose is to know what must be reproduced, retained, or deliberately changed when EMU Black replaces the current ECU architecture.

## Capture set C — PowerFC calibration reference

Before removing or materially changing the current PowerFC setup, preserve as much of the running calibration as practical.

Preferred evidence, in descending order of value:

1. A complete FC-Edit / FC-Datalogit `Read All` followed by `Save As` to preserve a native calibration file if the required interface/software is available.
2. Screenshots/exports of the complete ignition map, VVT map, fuel/base-injection map, map axes, VTLI/VVL changeover settings, rev/idle/boost-related limits, injector settings and correction data.
3. Useful datalogs from known-good operating conditions, especially idle, cruise, transient response, VVT/VVL transition and representative boost/load regions.
4. Handheld Commander values/screens only if more complete electronic extraction is not available.

Treat the PowerFC calibration as **REFERENCE**, not as a table-for-table EMU Black import source. The highest-value information is the behavior and shape of a tune already proven on this exact engine/car: ignition strategy, VVT strategy, VVL changeover, limits, idle targets/behavior, injector assumptions and useful logs. Fuel-table numbers are lower-confidence transfer material because the interim EMU stage also changes fuel-system hardware and may change injector characterization/fuel pressure; EMU Black also uses a different fueling model and correction structure.

Where values are preserved, record the original PowerFC table name/axis units and the car state/hardware they applied to. Do not silently reinterpret PowerFC units as EMU Black units.

## Capture set D — PowerFC datalog protocol

Goal: preserve repeatable operating behavior from the current known-good PowerFC calibration before ECU/fuel/DBW hardware changes. Use FC-Datalogit with the current wideband on AN1 only after verifying that AN1 scaling matches the actual wideband controller output. Preserve raw logs; do not rely only on screenshots.

Record before every log session:

- fuel in tank / fuel type;
- current boost-control hardware and boost setting;
- injector part number if known;
- fuel-pump configuration if known;
- ambient temperature if practical;
- engine coolant temperature at test start;
- whether A/C is on or off;
- gear used for each loaded test;
- any known mechanical or calibration issue.

Preferred logged channels where FC-Datalogit exposes them: RPM, PowerFC load/map cell, TPS, ignition timing, VVT command/value, injector duty or injector pulse information, knock value, coolant temperature, intake-air temperature, vehicle speed, battery voltage, wideband AFR/lambda, and any available airflow/MAF signal. Add boost/MAP only if an independent sensor is already available; do not modify the car solely for this capture.

### D1 — cold start and warm-up

Start after an overnight or genuinely cold soak with coolant near ambient.

- Begin logging before key-on/start.
- Start without touching the throttle unless the car actually requires intervention.
- Let the engine idle continuously through warm-up until coolant is at normal operating temperature and idle has stabilized.
- Keep A/C off for the primary warm-up capture.
- Record any stall, flare, hunting, manual throttle intervention or abnormal AFR behavior in the log notes.

### D2 — hot idle / accessory-load steps

With engine fully warm and stationary:

1. 30 s hot idle, A/C off.
2. Turn A/C on and hold 30 s.
3. Turn A/C off and hold 20 s.
4. If electrical loads are easy to reproduce, add headlights + cabin blower for 20 s, then remove them.

Do not manipulate the throttle during these steps unless needed to prevent a stall; note any intervention.

### D3 — hot restart

After full warm-up:

- shut engine off for 5 minutes;
- begin logging before restart;
- restart without throttle input;
- log at least 60 s after start with A/C off.

### D4 — steady-state cruise

On a safe, level road with engine fully warm, use the highest practical gear that does not lug the engine. Hold each condition as steadily as traffic allows for approximately 15–20 s:

- ~2000 rpm light-load cruise;
- ~2500 rpm light-load cruise;
- ~3000 rpm light-load cruise;
- ~3500 rpm light-load cruise if practical.

Repeat one of the midrange points with A/C on if road conditions permit. Avoid deliberately entering boost during the steady-cruise captures.

### D5 — light transient / tip-in

With engine fully warm and in a safe road environment:

- cruise steadily near 2000–2500 rpm;
- make three separate smooth throttle increases from light cruise to moderate acceleration, then return to cruise;
- do not target full boost or WOT;
- space events apart so the log clearly separates each transient.

The purpose is to capture acceleration enrichment / transient AFR response, not maximum performance.

### D6 — decel / fuel-cut behavior

With engine fully warm:

- establish a steady 3000–3500 rpm cruise in gear;
- fully release the throttle and remain in gear through engine braking down toward ~1500 rpm;
- perform two clean repeats if traffic permits.

Do not clutch in immediately after throttle closure; the goal is to capture overrun/fuel-cut entry and recovery.

### D7 — VVL / lift transition

Only when road/traction/mechanical conditions are suitable and the engine is fully warm:

- use one repeatable gear that allows the event to occur safely without excessive road speed;
- begin below 4500 rpm at moderate-to-high throttle;
- pass cleanly through the current 5600 rpm lift threshold and continue to approximately 6500 rpm;
- perform one or two clean repeats;
- do not continue to redline unless needed for the separate full-load reference below.

The purpose is to correlate PowerFC load, VVT strategy, timing, fueling and AFR across the lift transition.

### D8 — current full-load / boost reference

This is optional and must not be used to explore higher boost or higher load than the car has already been operating at safely.

Perform only if the current engine is healthy, fuel pressure/AFR behavior is known-good, traction and road/dyno conditions are controlled, and the existing boost setting is unchanged.

- engine fully warm;
- one repeatable gear suitable for a controlled pull;
- begin around 3000–3500 rpm;
- roll smoothly to full throttle;
- continue through lift and toward the normal high-rpm operating region;
- one clean pull is sufficient; a second is only for repeatability if the first is questionable;
- do not deliberately exceed the current tune's established boost/load envelope.

Abort immediately for unexpected lean AFR behavior, fuel-pressure loss if monitored, abnormal knock indication, misfire, detonation evidence, overheating, boost beyond the known setting, or any new mechanical symptom. The historical report that the current combination was capped by fuel-pump/injector capacity is itself a reason not to chase a cleaner or higher-load pull if the first log shows the system approaching its known limitation.

### Naming / preservation

Use descriptive filenames such as:

- `PFC_COLDSTART_YYYY-MM-DD.csv`
- `PFC_HOTIDLE_AC_YYYY-MM-DD.csv`
- `PFC_HOTRESTART_YYYY-MM-DD.csv`
- `PFC_CRUISE_2000-3500_YYYY-MM-DD.csv`
- `PFC_TIPIN_YYYY-MM-DD.csv`
- `PFC_DECEL_YYYY-MM-DD.csv`
- `PFC_LIFT_YYYY-MM-DD.csv`
- `PFC_FULLLOAD_CURRENTBOOST_YYYY-MM-DD.csv`

Preserve the untouched raw log first. Any cropped/annotated/derived analysis should be a separate file.

## Scope boundary

Do not delay the interim EMU conversion for open-ended BEAN/MPX reverse engineering or an attempted perfect PowerFC-to-EMU table translation. Capture enough baseline evidence to preserve the known-good reference state, then use the MWR Celica base map, the Lotus 2ZZ DBW reference map, actual hardware characterization and tuner validation to commission the EMU.

Cross-project research conclusions may also be summarized in `Celica-engineering-knowledge`, but executable capture work is owned by this Street Build repository.
