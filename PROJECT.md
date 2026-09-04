# Celica Street Build — Project State

**Vehicle:** 2000 US-spec Toyota Celica GT-S  
**Role:** finished street car and useful engineering exercise platform  
**Checkpoint:** 2026-09-04

## 1. Objective

The Street Build is intended to be the **last major drivetrain rebuild of the Celica**.

The finished car should be capable of roughly **500 whp** with reliability margin, but that figure is a capability target rather than the normal operating point. The normal personality should be a civilized low-boost street car that can be driven with passengers, used regularly, and enjoyed without treating the drivetrain as fragile.

After the Street Build, major progress should come from polishing the finished product: calibration, tires, suspension, alignment, sound system, seats, interior touch points, NVH, body refinements, wheel finish, and similar improvements. A future turbo change should be evolutionary rather than a reason to redesign the whole car.

## 2. Governing Principles

### Exploration does not imply commitment

The currently viable configuration remains the default until an alternative is explicitly selected. Owning a part or investigating an architecture does not make it a requirement.

### Build the hard interfaces once

Spend engineering effort on systems that are expensive to revisit after installation: drivetrain integration, controls, harnessing, turbo interfaces, fueling, service access, and major packaging.

### Use mature solutions where they are good enough

The Celica is not the heavy-R&D car. Proven parts and OEM-style solutions are preferred when they meet the requirement. Open-ended experimentation should earn its complexity.

### The spare subframe is the integration buck

The current car should stay intact and drivable as long as practical. Anything that can be resolved on the spare drivetrain package should be resolved there before the Celica comes apart.

The goal is **not** to build a second car on the floor. Pre-integrate the major drivetrain and control interfaces; ordinary supporting hardware can be disconnected from the current drivetrain and reconnected during the final swap.

### Optional experiments need an exit ramp

Opportunity work may be developed on the spare package, but it does not get to hold the car hostage. If an optional architecture is not mature when installation time arrives, use the known-good path.

## 3. Program Sequence

### Stage A — Baseline

Baseline is intentionally narrow and belongs to the separate baseline program:

- maintenance caught up;
- A/C sorted;
- power steering sorted;
- seats installed.

The result is a sorted current-powertrain street car: get in, drive, have fun.

### Stage B — Baseline Better / Independent Validation

Before the major drivetrain swap, upgrade systems that can be validated independently on the running engine with little downtime:

1. Install the return-style fuel system, pump, rail, and FPR.
2. Install EMU Black through the MWR adapter while retaining the current engine and simple cable-throttle configuration.
3. Have a tuner commission and tune the ECU.
4. Drive the car and validate the fuel system, ECU behavior, diagnostics, and tuner relationship.
5. Upgrade to appropriate tires before exploiting substantially more power.

This stage deliberately reduces the number of unknowns in the final build. If the current engine fails during tuning or validation, the already-prepared replacement drivetrain is the contingency rather than a new project starting from zero.

The MWR adapter is therefore a **commissioning tool**, not necessarily part of the finished architecture.

### Stage C — Replacement Drivetrain Build

Build the major package offline around:

- built 2ZZ;
- E153 transmission;
- MWR clutch/flywheel;
- MWR E153 mounts/adapters;
- MWR E153 axles;
- complete spare Celica subframe and rack;
- final turbo/intake/charge architecture once selected;
- final EMU Black controls package;
- full custom engine/control harness;
- DBW;
- flex fuel.

Resolve packaging, service clearance, major routing, sensors, mounts, and control interfaces on the spare package wherever practical.

### Stage D — Final Swap and Commissioning

The desired swap is closer to:

> Disconnect chassis interfaces → remove current drivetrain/subframe → install substantially complete replacement module → reconnect known supporting interfaces → fluids → startup/commissioning.

Supporting hardware does not need to be duplicated merely to make the floor assembly look complete.

## 4. Selected Final-Build Requirements

The following are current commitments:

- 2ZZ remains the engine architecture.
- Turbocharged street build remains the direction.
- EMU Black replaces the Apexi PowerFC.
- DBW is required in the final Street Build.
- Flex-fuel capability is required in the final Street Build.
- Full custom engine/control harness is the intended final electrical architecture.
- A/C must remain functional.
- Factory body/cluster functionality should be preserved where practical.
- The car should remain street-friendly, serviceable, and easy to live with.
- Approximate 500 whp maximum capability is the design envelope, not the default driving condition.
- Normal operation should support a conservative low-boost street map.
- Future major power changes should preferably be turbo/calibration changes rather than another drivetrain rebuild.

## 5. Electrical / Controls Strategy

### Interim

- EMU Black installed through the MWR adapter.
- Current engine and cable-throttle arrangement retained.
- Use the stage to validate ECU behavior, fuel delivery, tuning, and tuner trust.

The EMU Black has now been successfully bench-powered, connected, and migrated to **V3.061**. A 2ZZ reference calibration was imported from a V2 project; the resulting V2 -> V3 migration warnings are being treated as a controlled verification register rather than evidence that the imported map is ready to run the car. Bench trigger/synchronization characterization is now the next controls step before real-car installation. See [`EMU_COMMISSIONING.md`](EMU_COMMISSIONING.md).

### Final

- Remove the temporary adapter architecture.
- Wire EMU Black directly through a purpose-built custom harness.
- Add DBW, flex fuel, and final instrumentation/protection functions.
- Use the 2000 Celica EWD as the vehicle-year baseline and the 2005 Celica EWD deliberately where late 2ZZ hardware is selected.
- Define actual EMU I/O allocation before buying the broad final sensor package.
- Let the final sensor/protection strategy follow the real I/O map, available CAN expansion, and tuner input.

### Custom-harness connector verification

The first controlled connector-identification pass is underway. Unwired Ballenger Motorsports connector kits have been ordered for the currently expected OEM engine-device interfaces: crank/cam, VVT/VVL, VVTL/oil-pressure switch family, coolant temperature, late 2ZZ knock, ignition coil, late 2ZZ DBW throttle, accelerator pedal, and alternator control.

This purchase is a **verification set**, not a production harness order. Connector part numbers are supported by the 2000/2005 Toyota EWDs and supplier cross-references, but actual hardware fit, terminal/seal families, wire compatibility, and production quantities remain to be physically verified. The controlled register and verification procedure are maintained in [`HARNESS_CONNECTORS.md`](HARNESS_CONNECTORS.md).

The intended end state is a rebuildable full harness BOM that preserves device, housing, terminal, seal, wire, pin map, shielding/grounding, branch protection, tooling, and verification evidence without requiring chat history.

### Instrumentation / sensing direction

A dedicated external **MAP** and dedicated **EMAP/exhaust-pressure** measurement are part of the current sensing direction, with EMU Black's internal pressure sensor available for barometric/reference use if the final I/O strategy supports that arrangement. EGT provision is also desired for turbo/hot-side development and protection work.

Exact MAP/EMAP sensors, pressure ranges, EGT interface/channel count, oil/fuel pressure instrumentation, oil temperature, and other expansion channels are **not yet hardware-frozen**. Those choices remain downstream of final EMU I/O/CAN allocation and tuner review.

## 6. Known Replacement-Drivetrain Hardware

### Engine

Confirmed/recalled:

- sleeved 2ZZ block;
- upgraded connecting rods;
- forged low-compression pistons;
- ARP main hardware;
- ARP head studs;
- ARP rod bolts;
- OEM MLS head gasket;
- stock cams;
- upgraded valve springs;
- titanium retainers;
- new oil pump;
- new timing chain;
- new lift bolts;
- new thermostat;
- new water pump;
- Moroso upgraded oil pan;
- upgraded harmonic balancer currently being installed.

Tentative / verify from build records:

- compression ratio remembered as approximately **9.5:1**;
- valves are believed to be upgraded stainless valves;
- balancer is believed to be **ATI**;
- head is otherwise believed to be largely stock.

Before final assembly/commissioning:

- check and record valve clearance;
- verify compression ratio from build records;
- verify valve specification;
- verify balancer make/model.

### Transmission / driveline

- E153 transmission;
- factory LSD;
- MWR clutch/flywheel;
- MWR E153 mounts and adapter kit;
- MWR E153 axles.

The driveline is currently considered adequate for the intended build; no further paper design exercise is needed unless real evidence creates a reason to revisit it.

### Chassis integration

- complete spare Celica subframe;
- power-steering rack already installed on spare subframe;
- Mishimoto upgraded radiator is already installed on the current car and can remain a chassis-side supporting system.

## 7. Fuel System

Owned hardware:

- MWR return-style fuel system;
- MWR fuel rail;
- AEM fuel-pressure regulator;
- AEM fuel pump.

The fuel system is deliberately suitable for early installation on the current engine so it can be validated before the final drivetrain swap.

## 8. Turbo / Hot-Side Architecture

### Known-good fallback

The existing TurboKits.com T28-flanged architecture is usable. A straightforward T28-frame upgrade remains a credible low-risk path. If necessary, another TurboKits.com manifold can be purchased and the replacement drivetrain can be completed around that architecture.

### Modular sidewinder investigation

A preferred engineering concept is under investigation because it could make later turbo changes modular rather than architectural:

- driver-side / sidewinder turbo placement;
- compact stainless 2ZZ exhaust collector/manifold;
- EGT provisions;
- first-priority external-wastegate takeoff;
- collector terminated in a V-band;
- replaceable fabricated stainless up-pipe as the turbo-specific interface;
- ability to change turbo families later without redesigning the engine-side collector.

Cheap development hardware currently available includes a cast TD05-style manifold and inexpensive 20G turbo. These are **test/mule hardware**, not a selected final architecture.

The modular sidewinder concept is attractive, but it is not selected merely because it has been developed further than other alternatives.

A BorgWarner SX-E V-band turbo owned elsewhere is currently intended for the Civic, not this Celica build.

## 9. Intake / Charge Cooling

Open architecture:

- final DBW throttle body;
- OEM/off-the-shelf versus custom intake/plenum;
- final charge-cooling architecture;
- whether the existing TurboKits.com intercooler system remains or an A2W solution earns its complexity.

Owned development hardware includes Corolla 2ZZ intake runners and a 2003–2005 Celica DBW pedal.

## 10. Current Instrumentation

Current car:

- AEM Tru-Boost gauge/control on A-pillar;
- AEM wideband gauge on A-pillar.

Long-term preference is cleaner integration rather than a permanent collection of aftermarket gauges, but display architecture is intentionally deferred until the final control/data architecture is known.

## 11. Finished-Car Character

The Street Build should be able to do unreasonable things without behaving unreasonably all the time.

Desired normal experience:

- conservative low-boost mode available for routine street use;
- starts, idles, and drives predictably;
- A/C works;
- not excessively loud;
- not burdened by excessive road noise;
- good visibility;
- good sound system;
- comfortable, supportive seats;
- pleasant touch points;
- no constant concern that the drivetrain is made of glass.

The point of the high-power capability is reserve and fun, not an obligation to use maximum output every time the car leaves the garage.

## 12. Post-Build Evolution

Examples of desirable later refinement that should **not** require reopening the core drivetrain architecture:

- better suspension;
- better tires and chassis setup;
- alignment and calibration refinement;
- wheel powder coating / finish work;
- sound-system improvements;
- interior/touch-point refinement;
- carbon-fiber roof / sunroof-delete investigation;
- turbo swap within the modular interface if desired.

Ideas such as a 2GR with AWD transmission and a custom rear differential cradle are interesting future concepts, but they are outside the Street Build requirements and should not distort this project.

## 13. Current Open Decisions

Do not force these decisions before the work actually requires them:

- final turbo and hot-side architecture;
- whether the modular sidewinder earns selection over the known T28 path;
- final intake/plenum/throttle-body architecture;
- final intercooling architecture;
- exact custom-harness topology and EMU I/O allocation;
- final aftermarket sensor hardware/ranges, CAN expansion, protection logic, and display strategy;
- detailed chassis-side transfer list for final swap day.

The project should mature these decisions as evidence becomes available rather than pretending they are already frozen.
