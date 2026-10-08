# DBW Intake and Electronic Wastegate Mockup

**Checkpoint:** 2026-10-08

**Vehicle:** 2000 US-spec Toyota Celica GT-S / 2ZZ-GE

**Authority:** user-reported acquisitions and observations; engineering interpretations are labeled separately.

## Acquisitions and scope

| Hardware | Established state | Evidence / limit |
|---|---|---|
| OEM 2003–2005 Celica GT-S eTB | In hand; receipt previously reported 2026-10-01 | USER-REPORTED; exact label/part number and electrical validation still to record |
| Spare 2000–2002 Celica intake manifold | Bought from a Celica buddy for **$60**; available for mockup on the built engine | USER-REPORTED 2026-10-08; purchase date itself not specified |
| Electronic internal-wastegate actuator | Received; physically compared by hand with the MaXpeedingRods 20G | USER-REPORTED 2026-10-08; exact actuator model/part number not established in this update |

Recaro SR2 seats and Recaro rails are also received. Their acquisition, installation, and fit checks are owned by [celica-baseline](https://github.com/wildc4t-workshop/celica-baseline), not this project.

Hardware ownership does not select the final turbo, hot-side, intake, or boost-control architecture. The current Baseline Plus boost-control decision remains the Tru-Boost MAC solenoid driving the pneumatic wastegate under EMU control.

## Intake and eTB findings

**OBSERVED / USER-REPORTED — 2026-10-08:** the eTB bolt pattern matches the 2000–2002 intake manifold. This reduces the apparent mechanical adaptation scope. It does not establish bore/gasket alignment, sealing, motor clearance, connector clearance, hose routing, or working DBW control.

**HYPOTHESIS — user:** the later manifold revision may have angled the vacuum-hose connections upward to clear the electronic throttle motor. The factory revision history and reason have not been verified. Do not convert this explanation into a Toyota-confirmed design fact.

**TENTATIVE modification concept — user:** remove the existing hose connections and replace them with threaded ORB or AN fittings at the manifold. No fitting size, thread, removal method, or machining operation is selected yet.

The spare manifold provides an off-car integration buck and an opportunity to inspect the connection bosses without dismantling the running car. Before choosing a conversion, establish the existing connection construction, boss dimensions/material available, port routing, required sealing geometry, and actual clearance with the selected eTB. An ORB manifold port and an AN hose-end interface should be specified separately rather than treating their sealing arrangements as interchangeable.

Keep the remaining work within STREET-015: verify bore/gasket/sealing alignment, motor and hose/fitting clearance, connector access, and charge-pipe orientation. Record dimensions/photos and the resulting fitting decision. Do not add a bolt-pattern adapter merely because earlier planning assumed one might be needed; an adapter/spacer must solve a demonstrated remaining requirement.

## eWG findings

**OBSERVED / USER-REPORTED — hand-held visual comparison, 2026-10-08:**

- Overall size appears nearly the same as the pneumatic actuator.
- The motor assembly projecting from the side appears to be the main envelope increase.
- Held against the MaXpeedingRods 20G, the mounting flange appeared very close, possibly identical.
- Rod length also appeared very close, possibly identical.

These are useful packaging observations, not a measured dimensional match, a bolted-up fit check, or a powered bench test. No stroke, force, speed, feedback-voltage range, pinout, or power-loss position has been verified for the received unit. Earlier research candidates and other vehicles' scan-tool values must not be silently assigned to this actuator.

STREET-022 owns the next measurement: record the actuator label, mounting-hole centers and planes, rod-end geometry, closed/open endpoints, usable stroke, linkage alignment, and clearance through travel. Establish documented electrical requirements before powered testing. Useful validation evidence will include commanded versus measured position, movement direction, current demand, binding/stop behavior, and power-loss behavior. Holding force under exhaust load and hot-side heat exposure remain design gates beyond an unloaded fit check.

## Engineering value and limits

**MANUFACTURER / general principle:** Garrett identifies improved accuracy and response as electric-actuation benefits. Turbosmart describes position control independent of base spring pressure for its electronic wastegates. These sources support the control concept; neither establishes performance or ratings for the user's unidentified actuator.

**INFERRED application to this build:** a compatible motor/feedback/controller combination may allow low boost targets without accepting the reduced closing authority of a soft pneumatic spring. It can provide position feedback and commanded movement without waiting for compressor pressure. Those capabilities suit a conservative street calibration with higher-output modes, provided the actuator has adequate force and the controller is properly validated.

Limits remain:

- The internal bypass port and flap still determine bypass capacity; electric actuation does not cure a port-flow limitation or guarantee freedom from boost creep.
- Boost-by-gear is also possible with pneumatic control; electric actuation may improve usable range and authority rather than uniquely enabling the feature.
- No specific spool improvement, peak-power gain, or transient response has been measured on this car.
- Position feedback is actuator feedback; a detached linkage can invalidate any assumption about actual flap position.
- De-energized behavior must be characterized; do not assume the motorized assembly fails open or reproduces pneumatic base boost.

## EMU integration boundary

**MANUFACTURER:** EMU Black V3 documentation states support for electronic wastegate control. This is software-family capability, not proof that the project's installed 3.061 configuration, driver allocation, or received actuator is ready to use together.

The existing Baseline Plus allocation uses H-Bridge 1A/1B for the eTB and H-Bridge 2A for VVL. Do not assume a spare complete internal H-bridge is available for the eWG. Motor-current requirements, external-driver compatibility if needed, position-feedback input allocation, diagnostics, and controller behavior must be resolved explicitly. A high-current H-bridge alone is not evidence of a complete validated wastegate-position controller.

The eWG investigation is not added as a blocker to interim EMU commissioning. Changing the selected MAC/pneumatic path requires an explicit validated decision; preserve the current commissioning fallback.

## Provenance

- User acquisition and physical-observation report, 2026-10-08: SR2 seats/rails received; eWG received and visually compared to 20G; spare manifold purchased for $60; eTB bolt-pattern match; proposed vacuum-port explanation and threaded-fitting concept. No dimensioned drawing, photograph, invoice, or bench log was supplied with this update.
- Earlier user receipt report, 2026-10-01: OEM late-Celica eTB from RockAuto in hand. Exact label remains to record.
- [Garrett gasoline wastegate turbochargers](https://www.garrettmotion.com/turbocharger-technology/gasoline-turbochargers/wastegate-turbochargers-for-gasoline-engines/) — general electric-actuator accuracy/response benefit; reviewed 2026-10-08.
- [Turbosmart electronic PowerGate60](https://turbosmart.com/products/genv-electronic-ewg60-powergate60) — spring-independent position-control principle; an external-gate reference, not this actuator's specification; reviewed 2026-10-08.
- [EMU Black V3 migration guide](https://www.ecumaster.com/files/EMU_BLACK/EMU_BLACK_Migration_Guide_to_V3_Software.pdf) — Boost section documents electronic-wastegate support; reviewed 2026-10-08.
- [ECUMaster Dual H-Bridge](https://www.ecumaster.com/products/dual-h-bridge/) — candidate driver reference only, not an acquired or validated component; reviewed 2026-10-08.
