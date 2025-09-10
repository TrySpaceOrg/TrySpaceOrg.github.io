# DRM Concept of Operations

## System Overview

The Design Reference Mission (DRM) is a small, modular spacecraft model implemented and exercised inside the TrySpace Lab environment to teach, prototype, and validate flight operations and flight software concepts.

Key system components and their roles:

* cFS (Core Flight System): provides the onboard application framework, message bus, telemetry/command handling, event reporting (EVS), and standard apps used throughout the scenarios (ADCS, CFDP, DS, EVS, FM, etc.).
* Ground software (YAMCS): receives telemetry, displays spacecraft state, and provides command interfaces and scripted sequences (RTS) used during commissioning and nominal ops.
* Simulated hardware stacks: software models of ADCS, radios, payloads, and storage that behave like their flight counterparts within the constraints of TrySpace Lab.
* TrySpace Lab environment: provides a realistic ground-side operations environment built from containers and services (YAMCS for telemetry visualization and commanding, simulated radios, file servers, and the scenario stacks).

The DRM package in this atlas includes command sequences, scenario definitions, and documentation intended to exercise the interactions between these components during mission phases.

Mission subsystems:

* Attitude Determination and Control System (ADCS):
    * Purpose: Provides attitude sensing and control to point the vehicle (or payload) for mission objectives such as sun‑pointing, nadir observations, or stable inertial pointing.
    * Key interfaces: publishes attitude/quaternion telemetry, wheel rates, sun sensor readings and accepts mode, setpoint, and torque wheel/commanding messages via cFS topics.
    * Typical commands: mode changes (SUNSAFE, DETUMBLE, TRACK), attitude setpoints, calibration and sensor resets.
    * Failure modes & mitigations: loss of fine pointing (fall back to SUNSAFE), stuck wheel (command wheel desaturation or stop), sensor dropouts (switch to coarse sensors or safe mode).

* CryptoLib:
    * Purpose: Provides mission cryptographic primitives for secure storage, authentication, and encrypted telecommand/telemetry when enabled by the mission profile.
    * Key interfaces: offers crypto services to other cFS apps (sign/verify, encrypt/decrypt, key management hooks) and may log audit events to EVS/DS.
    * Typical commands: key provisioning (simulated), enable/disable crypto services, perform self-tests.
    * Failure modes & mitigations: key corruption or service failure (fail closed or open per mission policy); in the lab, operators may bypass or reset the crypto service if needed for recovery.

* Demonstration Instrument:
    * Purpose: A representative payload used to exercise data collection, on‑board storage, and retrieval workflows (e.g., generate files that are managed by FM/DS and transferred via CFDP).
    * Key interfaces: configuration commands (integration time, gain, mode), science telemetry, and file output into the on‑board file system.
    * Typical commands: initialize/enable instrument, configure measurement parameters, start/stop acquisitions, and request data product generation.
    * Failure modes & mitigations: stuck acquisition (reset instrument app), overflow of on‑board buffers (request immediate downlink or clear files), corrupted files (re-run acquisition or mark for discard).

* Electrical Power System (EPS):
    * Purpose: Manages generation (simulated solar arrays), storage (batteries), power distribution, and thermal constraints required to operate the spacecraft.
    * Key interfaces: publishes bus voltages, battery state of charge, solar panel outputs, load states, and accepts power mode and load shed commands.
    * Typical commands: change power modes, enable/disable loads, perform battery/state-of-charge resets, and run power health tests.
    * Failure modes & mitigations: low bus voltage (shed non‑essential loads, enter safe mode), battery overtemp (reduce charging), simulated panel degradation (update mission ops plan to reduce power budget).

* Radio:
    * Purpose: Simulates the space‑to‑ground and ground‑to‑space communication link including pass scheduling, duplex/half‑duplex modes, and point‑to‑point data transfers.
    * Key interfaces: radio state and mode telemetry, scheduler hooks for RTS start/stop, and command interfaces for mode/config changes used by the ground (YAMCS/UI).
    * Typical commands: configure radio mode (DUPLEX/RECEIVE/TRANSMIT), start/stop simulated passes (RTS), and set link parameters (data rate, modulation in advanced scenarios).
    * Failure modes & mitigations: link outage (retry on next pass or extend pass with RTS), packet loss (retransmit via CFDP), and misconfiguration (reset radio or re‑issue pass sequence).

## Mission Phases

1. Launch & Deployment

* Immediately after separation the vehicle is placed into a conservative, power‑saving "safe mode." 
* No component subsystems are enabled and the spacecraft is tumbling.
* The data storage (DS) application is logging all event messages and telemetry produced to file.
* The limit checker (LC) and stored command (SC) applications perform any fault detection and correction.
* The basic initialization is done in cFS via the startup relative time sequence (RTS) number 1.
* The spacecraft awaits ground contact for commissioning.

2. Commissioning

* Establish first contact from the ground by sending the Start RTS 6 (Start Pass) command.
* Verify command/telemetry link and basic responsiveness with a NOOP command and other health checks.
* Bring core subsystems online (ADCS, power management, payload) following verified checklists.
* Perform basic functional tests (file listings via FM, file transfers via CFDP, storage management via DS).
* Perform any anomaly investigations as necessary.

3. Nominal Operations

* Regular science/payload activities, attitude maintenance, periodic housekeeping, and scheduled data downlinks.
* Modifications to limits and configuration settings as requested by subsystems after review.
* Routine use of RTS on-board the vehicle and YAMCS procedure stacks to automate passes.

4. Contingency and Anomaly Response

* Detection via EVS/event messages and telemetry limits.
* Fault isolation using cFS app diagnostics, log review, and replayed telemetry in TrySpace Lab.
* Recovery actions: safe mode recovery procedures, selective subsystem restarts, and uplinked software patches where applicable.

5. Decommissioning / End of Life

* Final data retrieval, graceful shutdown of mission services, and transition to a passive or disposal state according to mission rules.

## A Day in the Life (Nominal Operations)

A typical operations day for the DRM in TrySpace Lab focuses on monitoring, scheduled commanding, and data handling.
Depending on the current data rates one or more passes per day are expected, assuming the ground station is available for scheduling.
Assuming one pass the following is to be performed:

* Confirm current state of health, begin anomaly investigation if off-nominal.
* Perform any uploads or configuration changes.
* Delete any files that the data pipeline confirmed successful receipt of.
* Get the latest data file set from on-board the vehicle.
* Download data files until the end of the pass.
* Once pass is complete confirm data pipeline is has begun processing.

It is typical for data to be backlogged on the vehicle.
Data may be requested to be skipped based on external phenomenon.

## Operational Constraints and Simulation Fidelity

The TrySpace Lab DRM scenario provides high fidelity in command/telemetry flows and flight software behavior, but the operator should be aware of limitations:

* Timing and pass windows are simulated. Real orbital dynamics are being calculated and used by the simulators, but are not leveraged for pass durations.
* Hardware effects and failures are modeled at a software level; some low-level hardware dynamics (thermal gradients, fine star‑tracker jitter, radiation effects) are either simplified or omitted depending on the scenario.
* Resource contention and container-level resource caps (CPU, memory) are host-dependent and may affect simulated timing when running on limited machines.
* Network latency and Docker host performance can change CFDP transfer characteristics compared to in-flight performance.

Recommended operator practices to mitigate these constraints:

* Use the scenario stacks and stepwise execution controls in YAMCS to reproduce timing-sensitive steps deterministically.
* When validating fault responses, explicitly inject failures through the scenario controls rather than relying on emergent, hard-to-reproduce conditions.
* Monitor host resources (docker stats) while running heavy scenarios and consider increasing container resources for high‑fidelity testing.

## Roles and Responsibilities

This section clarifies who does what in the TrySpace Lab + DRM workflow.

* Flight Controllers (Mission Operator)
  * Primary responsibility for commanding and monitoring the DRM during commissioning and nominal operations.
  * Executes RTS sequences, performs health checks, and coordinates data downlinks.

* Subsystem Leads (ADCS, Payload, Radio)
  * Expert for a specific subsystem. Runs detailed checkouts, interprets subsystem telemetry, and advises the Flight Controller on configuration changes.

* Simulation Engineer / Lab Maintainer
  * Maintains the TrySpace Lab scenario stacks, container images, and scenario scripts used by the DRM.
  * Ensures YAMCS, CFDP, and the simulated hardware stacks are up-to-date and reproducible.

* Software Engineer (cFS & Apps)
  * Develops and maintains cFS applications and ensures their integration with the simulation environment.
  * Produces and validates command/telemetry definitions and app-level test cases used in the lab.

## Success Criteria and Acceptance Tests

To consider a mission phase or daily operations successful in the DRM context, the following acceptance criteria are used:

* Commissioning: Ground successfully executes the Start Pass RTS, issues a NOOP, and observes incremented command counters and expected EVS messages.
* Subsystem Checkout: ADCS and demo instrument initialize, accept configuration commands, and report expected telemetry values for the configured modes.
* Data Handling: Files listed by FM are retrievable via CFDP and stored locally on the ground; DS successfully closes file sets for downlink.

## Notes and Next Steps

See the commissioning scenario (`atlas/docs/scenarios/commissioning.md`) for a step‑by‑step walkthrough that implements the checks and sequences summarized here.

Last updated: 20250910
