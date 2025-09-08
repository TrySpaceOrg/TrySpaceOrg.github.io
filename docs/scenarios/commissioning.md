# DRM Commissioning

## Objective

In this scenario, you will perform the initial "on-orbit" checkout of the Design Reference Mission (DRM) spacecraft.
As the flight controller, your job is to make first contact, verify the health and status of the spacecraft's core systems, and transition it from its post-launch "safe mode" into a fully operational state, ready for nominal operations.

## Prerequisites

Before you begin, please ensure you have:

* Successfully completed [Getting Started](../manual/handbook/getting-started.md).
* The TrySpace Lab environment is installed and able to run.
* Read the [DRM Concept of Operations](../drm/concept-of-operations.md) to understand the goals of the spacecraft you'll be controlling.
* (Optional) Reviewed the [System Architecture](../manual/core-concepts/architecture.md) page to understand how the components interact.

## Overview

The DRM spacecraft has just been deployed from its launch vehicle.
It is currently in a power-saving, thermally stable "safe mode."
In this state, only essential components are active, and it is broadcasting basic health telemetry while awaiting its first commands from the ground.
Your task is to walk through the commissioning checklist to bring it to full functionality.

We will follow these four phases:

* Making first contact: establish the space link.
* Initial health assessment: verify the spacecraft is healthy after launch.
* Subsystem checkout: power on and configure the core subsystems.
* Transition to nominal mode: command the spacecraft into its final operational state.

## Startup

Launch TrySpace Lab:

* Open a terminal
* Navigate to your tryspace-lab repository - `cd tryspace-lab`
* Launch - `make start`

...

Open the YAMCS ground software: [localhost:8090](http://localhost:8090)

...

Verify everything is running:

* Confirm time is incrementing in the primary terminal window.
* Ensure data is flowing through the YAMCS debug interface.
* Check that expected containers all exist and are operational.

...

## First Contact

Command the "Start Pass" relative time sequence (RTS).
This enables the radio for 8 minutes simulating a long pass if the space vehicle was in a Low Earth Orbit (LEO).
You should see FSW print receipt of the command, and the radio-in link receiving data in YAMCS.

...

You've now successfully commanded and are receiving telemetry from your DRM spacecraft!

## Health Assessment

Open the DRM display and confirm values within limits.

...

Test the command link with a CFS_ES_NOOP_CC, the "hello world" of the cFS Flight Software.

...

Ensure the command counter incremented.

## Subsystem Checkout

### ADCS

Enable ADCS and confirm health.

Set to sun safe mode.

Confirm sun pointed (X+ axis).

### Demo Instrument

Enable demo instrument and confirm health.

Set configuration.

Confirm data flow.

## Transition to Nominal Mode

Check what data exists on the space vehicle using the File Manager (FM) application in cFS.

Download data from the space vehicle using the CCSDS File Delivery Protocol (CFDP) application in cFS.

Upload a new initialization RTS to enable components and set ADCS to sun safe mode.

## Review

Congratulations! 
You have successfully completed the commissioning of the DRM spacecraft. 
You have:
* Established a stable command and telemetry link.
* Verified the health of the spacecraft's core systems.
* Powered on and configured the ACS and primary payload.
* Transitioned the vehicle from a post-launch safe state to being fully mission-ready.

Next steps include performing day-to-day [nominal operations](./nominal-operations.md).
