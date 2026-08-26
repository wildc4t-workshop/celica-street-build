# AGENTS.md — Celica Street Build

## Mission

This repository is the engineering system of record for the **final major drivetrain and controls build** of the 2000 US-spec Toyota Celica GT-S.

It consolidates work that was previously staged as separate turbo/hot-side, charge-cooling, EMU Black, engine-harness, instrumentation, BEAN/cluster, intake/DBW, and packaging projects. Do **not** recreate those as separate active repositories unless the program architecture is deliberately changed.

The Street Build must remain understandable without chat history.

## Core operating rule

**Markdown is durable engineering memory. `tasks.csv` is engineering attention. `project.yaml` is machine-readable project state. The dashboard is derived only.**

Do not leave durable conclusions only in chat, tasks, commit messages, ECU files, CAD, or tuner notes. Convert useful outcomes into the repository.

## Read before changing state

Before making technical or project-state changes, read at minimum:

- `PROJECT.md`
- `tasks.csv`
- `project.yaml`
- any topic-specific document that exists for the affected subsystem
- relevant vehicle-wide reference material in `Celica-engineering-knowledge` when year-specific factory behavior matters

Treat current repository state as authoritative unless the user is explicitly correcting it.

## Collaboration rule

The user may provide natural-language status updates from any chat, for example:

- `Street Build: the tuner is selected.`
- `The fuel rail is installed.`
- `That sensor is actually stock, not upgraded.`

Do not require the user to know task IDs, filenames, or CSV structure. Read the repository, resolve the affected state, update durable documentation/tasks as appropriate, and report what changed.

If the user says not to update GitHub yet, discuss only.

## State and evidence discipline

Classify information as appropriate:

- fact / observation
- inference
- tentative direction
- selected decision
- rejected / superseded decision
- open question
- executable task

Useful evidence labels include `MEASURED-CAR`, `BENCH-TESTED`, `FACTORY-DOC`, `MANUFACTURER`, `COMMUNITY-CORROBORATED`, `CAD-DERIVED`, `INFERRED`, and `TENTATIVE`.

**Owning hardware does not imply architectural commitment. Exploration does not imply selection.**

For the 2000 GT-S, do not silently assume later 2002–2005 2ZZ/Celica wiring, sensors, or network behavior is identical.

## Task and decision IDs

Use consolidated Street Build task IDs:

`STREET-###`

Use decision IDs:

`DEC-STREET-###`

The historical subsystem prefixes (`TUR-`, `A2W-`, `EMU-`, `HAR-`, `SNS-`, `NET-`, `PKG-`, `INT-`) came from the abandoned one-repository-per-subsystem architecture. Do not create new tasks under those prefixes.

Canonical task schema:

```text
id,title,status,action,time_min,context,cost,priority,blocked_by,decision_needed,doc_link,requires_car_down,requires_parts,notes
```

Statuses: `backlog`, `ready`, `doing`, `blocked`, `verify`, `done`.

Keep `tasks.csv` limited to currently useful executable work. Do not turn every design thought, owned part, or future possibility into a dashboard task.

## Governing program architecture

Protect these selected principles unless new evidence explicitly changes them:

- This is intended to be the **last major drivetrain rebuild** of the Celica.
- 2ZZ remains the engine architecture.
- Turbocharged street build remains the direction.
- EMU Black replaces the Apexi PowerFC.
- DBW and flex fuel are required in the final build.
- A purpose-built custom engine/control harness is the intended final architecture.
- A/C must remain functional.
- Preserve factory body/cluster functionality where practical.
- Roughly 500 whp is a **maximum capability/design envelope**, not the default map.
- Normal operation should be conservative, predictable, street-friendly, and serviceable.
- Keep the current car usable as long as practical.
- Use the spare subframe and replacement drivetrain as the integration buck.
- Pre-integrate major hard interfaces off-car; do not build a redundant second car on the floor.
- Optional experiments require an exit ramp and may not hold the car hostage.
- Prefer mature/OEM/off-the-shelf solutions where they satisfy the requirement; reserve custom engineering for places where it adds real value.

## Staged controls/fuel strategy

Do not collapse the interim and final electrical architectures.

### Interim commissioning

- Install and validate the return-style fuel system on the current engine.
- Install EMU Black through the MWR adapter.
- Keep the current cable-throttle/simple current-engine configuration.
- Use the running car to validate fuel delivery, ECU behavior, diagnostics, tuning, and the tuner relationship.

The MWR adapter is a commissioning tool, not the intended finished harness architecture.

### Final

- Built 2ZZ + E153 on the spare subframe.
- EMU Black direct integration through a full custom harness.
- DBW.
- Flex fuel.
- Final turbo/intake/charge architecture once selected.
- Final instrumentation/protection functions after the real I/O map is known.

## EMU Black / controls discipline

Maintain one authoritative final I/O allocation when that work becomes mature enough to freeze.

For every allocated channel, preserve device, signal type/range, power/ground reference, calibration source, failure behavior, and verification status.

Do not allocate inputs informally or buy a broad sensor package before required control functions, DBW/flex/turbo architecture, available I/O/CAN expansion, and tuner input are understood.

Protection logic should document sensor validity, thresholds, hysteresis/delay, action, and recovery behavior rather than inventing unsupported numbers.

Commission progressively: power/grounds -> communications -> sensor sanity -> actuator checks -> cranking sync -> fuel/ignition -> DBW -> idle/VVT/VVL -> boost -> advanced strategies.

## Harness discipline

The final harness must be rebuildable/serviceable from documentation.

For every device connector preserve year applicability, housing/series when known, terminal and seal families, wire compatibility, source/confidence, and actual-car verification status.

Do not call a connector identified from appearance alone when the terminal ecosystem remains uncertain.

Maintain controlled pin maps, grounding architecture, splice strategy, shielding/twisted-pair requirements, relay/fuse ownership, strain relief, heat/abrasion protection, and crimp tooling information.

Coil and injector sub-harnesses may be used where they improve modularity/serviceability.

## Factory cluster / BEAN / MPX discipline

The current PowerFC behavior is evidence, not proof of EMU compatibility. The MIL currently works and the cluster appears broadly functional; preserve actual-car observations before removing the baseline ECU architecture.

For captures/characterization, record vehicle state, ignition/engine state, exact test point, voltage levels, sampling settings, triggered action, log filename, repeated observations, and controls/baselines.

Prefer passive observation before active injection. Do not treat BEAN/MPX as standard CAN merely because CAN tools are available.

Maintain signal ownership for functions such as tach, coolant display, MIL, vehicle speed, charging warning, A/C interactions, fan control, and diagnostic/body dependencies.

## Turbo / hot-side discipline

The **known viable fallback** remains the existing TurboKits.com T28-flanged architecture.

The modular sidewinder architecture is an investigation, not a selected final architecture. Cheap cast TD05-style hardware and the inexpensive 20G are test/mule hardware.

If sidewinder work continues, optimize globally for:

- A/C and power-steering retention
- belt/accessory and radiator service
- oil-drain geometry
- wastegate priority/control
- downpipe and charge routing
- engine movement
- thermal protection
- fastener/tool access
- turbo removal path
- future modular turbo changes

A component may validate geometry without becoming the final component.

## Intake / DBW discipline

DBW is selected; the exact throttle body/plenum architecture is not.

Corolla 2ZZ runners and custom printed/aluminum-plenum concepts are available development paths, but a proven OEM/off-the-shelf DBW solution remains acceptable if it satisfies the requirement.

Do not select a throttle body solely by bore/bolt pattern. Verify redundant position sensing, electrical compatibility, actuator current/control requirements, calibration, and failsafe behavior with EMU Black.

Printed prototypes validate packaging/assembly, not boost-pressure durability.

## Charge cooling / packaging discipline

A2W is optional, not a requirement. The existing TurboKits.com intercooler system remains a viable baseline unless a different architecture earns its complexity.

For any charge-routing or cooling design preserve geometry sources, keep-out zones, engine movement allowance, A/C/PS clearance, hose/wire clearance, wrench access, assembly/removal path, thermal constraints, and support strategy.

Low-cost scans are useful for packaging but are not metrology truth for critical hardpoints without confirmation.

Do not choose AM, composites, printed metal, Wiggins/HD clamps, V-bands, or welded fabrication for novelty. Record the requirement each manufacturing choice solves.

## Instrumentation discipline

Current car has an AEM Tru-Boost gauge/control and AEM wideband gauge.

Long-term display/sensor architecture is intentionally deferred. Oil pressure, oil temperature, fuel pressure, EGT, exhaust pressure, and similar channels are **candidates**, not frozen requirements.

Sequence instrumentation work as:

1. define required control/protection functions;
2. establish final EMU I/O map and expansion options;
3. identify sensors that materially support protection, tuning, diagnosis, or verification;
4. review with the tuner;
5. select permanent vs development-only sensors and display strategy.

Do not buy sensors merely because an input appears available.

## Cross-project boundaries

- **Celica Baseline** owns current-car maintenance, A/C restoration, hydraulic PS restoration, seats, and current-car charge-pipe refinement.
- **CeliKey** owns passive entry/body-control/keyless-start R&D.
- **Celica Side Projects** owns BBK and EPS.
- **Celica-engineering-knowledge** preserves shared factory/reference knowledge.

Create explicit dependencies when another repo owns the work rather than duplicating tasks.

## Definition of done

A task is not done merely because activity occurred. Before setting `done`:

1. preserve the useful result in durable documentation;
2. update affected decisions/current architecture;
3. add only genuinely useful follow-on tasks;
4. reconcile dependencies and newly ready work;
5. preserve test/CAD/log/source references where relevant;
6. update maturity if the project state materially changed.

## End-of-session reconciliation

After meaningful engineering work, ask:

- What fact was established?
- What assumption changed?
- What decision changed or remains open?
- What needs verification?
- What task became ready/blocked/done?
- Did another project inherit a dependency?
- Is the durable record sufficient to resume months later?

Do not paste chat transcripts as engineering state. Convert them into concise, current technical memory.
