# Porting research plan

## Project stages

### Stage 1 — Baseline and source inventory

- Confirm the exact Moto G60 model, installed Android version, firmware and kernel baseline.
- Audit available hanoip device trees and Motorola kernel sources.
- Record source revisions, licenses and maintenance status.
- Keep all findings reproducible and cite upstream sources.

### Stage 2 — Samsung reference research

- Select an official One UI 9 reference device/build.
- Verify its Android platform version from authoritative documentation.
- Record firmware metadata and component boundaries.
- Review licenses before reusing or redistributing any material.

### Stage 3 — Compatibility analysis

- Map framework, system service, permission, SELinux and VINTF requirements.
- Compare Android platform requirements with the Moto G60's available vendor and kernel interfaces.
- Identify features that need original compatibility work.
- Classify each finding as compatible, uncertain, incompatible or not yet tested.

### Stage 4 — Development environment

- Document Windows, PowerShell and WSL2 versions.
- Create a reproducible Linux research environment.
- Keep scripts small, documented and non-destructive by default.
- Do not run device-modifying commands as part of environment setup.

### Stage 5 — Controlled prototypes

Only after source and compatibility reviews, consider isolated, reversible experiments. Do not publish a flashable image or claim device support without repeatable testing on the exact XT2135-2 variant.

## Feature tracking

| Area | Status | Evidence required |
|---|---|---|
| Wi-Fi | Not tested | Reproducible device test |
| Battery | Not tested | Charging, reporting and suspend tests |
| Audio | Not tested | Playback, recording and routing tests |
| GPU | Not tested | Driver and rendering validation |
| CPU | Not tested | Scheduler, frequency and thermal validation |
| Apps | Not tested | App-by-app compatibility results |
| Bluetooth | Planned / not tested | Pairing, audio and data tests |
| Camera / sensors / cellular | Not tested | Hardware-specific validation |

## Release gate

A build must not be described as installable or usable until its source provenance, exact target variant, build process, recovery strategy and test results are documented. No milestone is complete based only on code compiling.
