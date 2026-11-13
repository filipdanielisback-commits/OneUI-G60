# OneUI-G60

An experimental research and compatibility project exploring whether genuine, legally redistributable Samsung One UI 9 components can be adapted for the **Motorola Moto G60 (XT2135-2, 6 GB RAM)**.

> **Status: Research / planning. No installable ROM is available.**
>
> This repository does not claim that One UI 9 currently boots on the Moto G60 or that any hardware feature is working. Compatibility claims must be supported by reproducible tests on the exact target device.

## Target device

| Item | Target |
|---|---|
| Manufacturer | Motorola |
| Device | Moto G60 Vision (Motorola regulatory model designation for XT2135-2) |
| Model | XT2135-2 |
| Shared development codename | hanoip |
| RAM variant | 6 GB |
| Reported firmware/build | S2RIS32.32-20-9-9-2-3 |
| Reported Android security update | November 1, 2023 |\n| User-reported kernel | 4.14.190-perf+ (build timestamp: Fri Oct 27 13:02:24 CDT 2023) |
| Project target | Experimental One UI 9 compatibility research |

The firmware/build and security patch details are user-reported and should be checked against the device's About phone screen before being treated as independently verified.

## Areas of investigation

- **Android framework:** Android base version, framework APIs, services, permissions, SELinux policy and Samsung framework dependencies.
- **Vendor / HAL:** Motorola device-specific vendor implementation, HAL interfaces and VINTF compatibility.
- **Kernel:** exact device kernel source, configuration, drivers and supported interfaces.
- **Hardware:** Wi-Fi, battery, audio, GPU, CPU, camera, sensors, mobile network, fingerprint and Bluetooth.
- **Samsung components:** provenance, version compatibility, licensing and redistribution permissions for each component.

## Feature status

Nothing is marked as working until it has been tested on an XT2135-2 and the test evidence is documented.

| Component | Status |
|---|---|
| Wi-Fi | Not tested |
| Battery | Not tested |
| Audio | Not tested |
| GPU | Not tested |
| CPU | Not tested |
| App compatibility | Not tested |
| Bluetooth | Not tested; planned future work |
| Camera / sensors / cellular / fingerprint | Not tested |

## Repository layout

- `docs/` — device research, compatibility notes and safety guidance
- `device/motorola/moto-g60/` — future device-specific configuration and documentation
- `framework/` — framework research and original compatibility work
- `vendor/` — vendor interface research; do not commit restricted binaries
- `kernel/` — kernel source references and original configuration work
- `hardware/` — hardware test plans and results
- `patches/` — original patches with provenance
- `testing/` — reproducible test procedures and reports

## Safety and legal boundaries

- Do not commit proprietary Samsung or Motorola firmware, vendor blobs, restricted binaries, private signing keys or other material without permission to redistribute.
- Prefer links to upstream sources, version identifiers and original documentation.
- Do not flash experimental images on a primary device. Keep a verified recovery path and backups before device testing.
- Never describe an untested feature as working.
- Do not use Samsung branding in a way that suggests this is an official Samsung project.

## Contributions

Issues and pull requests are welcome for device research, source references, documentation, compatibility analysis and reproducible tests. Include the exact device model, software build, test steps and logs with sensitive identifiers removed.

## Disclaimer

OneUI-G60 is an independent community research project and is not affiliated with, endorsed by, or supported by Samsung or Motorola. Samsung One UI and related trademarks are the property of their respective owners.


## Current research milestone

The shared `hanoip` device-tree references have been located, but the community target index marks the target as dropped. Motorola's exact-build kernel-source request is closed and marked as published; however, the exact `S2RIS32.32-20-9-9-2-3` kernel branch has not yet been identified. See [device research and kernel audit](docs/DEVICE_INFORMATION.md) before treating any source as a build base.
