# AGENTS.md — Celica Street Build

## Mission

This repository is the engineering system of record for the **final major drivetrain and controls build** of the 2000 US-spec Toyota Celica GT-S.

Do not recreate historical subsystem repositories for turbo, intake, EMU, harness, instrumentation, or packaging unless the program architecture is deliberately changed.

The Street Build must remain understandable without chat history.

## Core operating rule

**Markdown is durable engineering memory. `tasks.csv` is engineering attention. `project.yaml` is compact machine-readable state. The dashboard is derived only.**

Do not leave durable conclusions only in chat, task notes, commits, ECU files, CAD, or tuner memory.

Before changing state, read at minimum:

- `PROJECT.md`;
- `tasks.csv`;
- `project.yaml`;
- the affected topic document;
- relevant factory/reference material when year-specific behavior matters.

Treat current repository state as authoritative unless the user explicitly corrects it.

## Evidence / decision discipline

Classify information appropriately:

- fact / observation;
- inference;
- tentative direction;
- selected decision;
- rejected / superseded decision;
- open question;
- executable task.

Useful evidence labels include `MEASURED-CAR`, `BENCH-TESTED`, `FACTORY-DOC`, `MANUFACTURER`, `COMMUNITY-CORROBORATED`, `CAD-DERIVED`, `INFERRED`, and `TENTATIVE`.

**Owning hardware does not imply architectural commitment. Exploration does not imply selection.**

Do not silently assume 2002–2005 Celica wiring/sensors/network behavior is identical to the 2000 GT-S.

## Task discipline

Use `STREET-###` task IDs and `DEC-STREET-###` decision IDs.

Canonical task schema:

```text
id,title,status,action,time_min,context,cost,priority,blocked_by,decision_needed,doc_link,requires_car_down,requires_parts,notes
```

Statuses: `backlog`, `ready`, `doing`, `blocked`, `verify`, `done`.

Keep `tasks.csv` limited to useful executable work. Do not turn every design thought or owned part into a dashboard task.

## Governing program architecture

Protect these selected principles unless new evidence changes them:

- this is intended to be the last major drivetrain rebuild;
- 2ZZ remains the engine architecture;
- turbocharged street build remains the direction;
- EMU Black replaces PowerFC;
- DBW and flex fuel are required;
- final architecture uses a purpose-built engine/control harness with the EMU in the cabin;
- A/C remains functional;
- practical factory body/cluster behavior should be preserved;
- ~500 whp is a maximum design envelope, not the default map;
- normal operation should be conservative, predictable, and serviceable;
- keep the current car usable as long as practical;
- use the spare subframe/replacement drivetrain as the integration buck;
- optional experiments require an exit ramp;
- prefer mature/OEM/off-the-shelf solutions where they meet the need.

## Stage ownership

### Baseline Plus — interim current-engine commissioning

Authoritative architecture: [`BASELINE_PLUS.md`](BASELINE_PLUS.md).

Selected direction:

- MWR adapter/PCB remains the interim Celica/body integration bridge;
- removable jumper/sub-harness carries modifications;
- late-Celica OEM DBW is pulled forward if ETB fit/bench validation succeeds;
- H-Bridge 1A/1B -> ETB motor;
- AUX6 -> VVT after verified OCV power/low-side conversion;
- H-Bridge 2A -> VVL;
- Radium 20-0589 + GM/Continental 13507129 -> flex fuel;
- Link MIPS 101-0325 -> fuel pressure and oil pressure;
- Bosch LSU 4.9 -> native EMU wideband;
- existing Tru-Boost MAC valve -> G22 / Injector 6 for EMU boost control;
- current MAF/IAT and internal-MAP hose may remain temporarily where that avoids scope.

Do not add final-build sensor development to Baseline Plus unless it materially helps current-engine commissioning.

### Final build

Authoritative sensor direction: [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md).

- built 2ZZ + E153 on spare subframe;
- MWR adapter removed from final control architecture;
- full custom engine/control harness;
- EMU mounted in cabin;
- MAF deleted;
- local external MAP near throttle/manifold interface;
- dedicated post-intercooler IAT or justified TMAP;
- CAN expansion used primarily for secondary/development channels such as EMAP, multi-channel EGT, oil temp, coolant pressure, etc.

Do not move proven critical direct signals onto CAN merely to make the architecture visually uniform.

## EMU / commissioning discipline

Authoritative commissioning record: [`EMU_COMMISSIONING.md`](EMU_COMMISSIONING.md).

Keep the two reference-map roles separate:

- recovered MWR supercharged-2ZZ Celica map = MWR/Celica I/O and integration reference;
- Lotus 2ZZ DBW map = DBW implementation reference.

Neither is a validated tune for this car.

Commission progressively:

> power/grounds -> communication -> trigger sync -> sensor sanity -> output ownership -> fuel/ignition -> DBW -> idle/VVT/VVL -> boost -> protections -> high load

For protection logic, document sensor validity, thresholds, hysteresis/delay, action, and recovery behavior. Do not invent unsupported threshold values.

## Harness discipline

Authoritative connector register: [`HARNESS_CONNECTORS.md`](HARNESS_CONNECTORS.md).

The final harness must be rebuildable/serviceable from documentation. Preserve for every production connector/device:

- year/application basis;
- housing/family;
- terminal/seal and wire compatibility;
- pin map;
- wire type/gauge;
- sensor/power-ground ownership;
- splice strategy;
- shielding/twisted-pair needs;
- fuse/relay ownership;
- strain relief and heat/abrasion protection;
- crimp/extraction tooling;
- evidence/verification state.

Do not call a connector production-ready from appearance or catalog cross-reference alone.

## Factory body / MPX discipline

The current PowerFC behavior is evidence, not proof of EMU compatibility.

Before removing the known-good architecture, capture the minimum evidence defined in [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md).

Prefer passive observation before active injection. Do not treat Toyota MPX/BEAN as standard CAN merely because CAN tools are available.

## Turbo / intake / charge discipline

Known hot-side fallback: existing TurboKits.com T28-flanged architecture.

Modular sidewinder remains an investigation, not a selected final architecture. Cheap TD05-style manifold/20G hardware are development aids.

DBW is selected, but the purchased 2003–2005 Celica GT-S ETB still requires physical fit and electrical validation on the 2000 manifold.

A2W remains optional. Existing TurboKits.com intercooling remains a viable baseline.

Do not select fabrication methods, clamps, printed metal, composites, or custom manifolds for novelty. Record the requirement each custom choice solves.

## Instrumentation discipline

Baseline Plus hardware already selected for control/protection should not be described as merely a candidate:

- Link fuel pressure;
- Link oil pressure;
- LSU 4.9;
- Continental flex fuel;
- MAC boost-control solenoid.

Final secondary/development instrumentation remains open and should be selected only when it materially supports protection, tuning, diagnosis, or validation.

## Cross-project boundaries

- `celica-baseline` — current-car maintenance, A/C, hydraulic PS, seats, current mechanical baseline.
- `CeliKey` — passive entry/body-control/keyless-start R&D.
- `celica-side-projects` — BBK and EPS.
- `Celica-engineering-knowledge` — shared factory/reference knowledge.
