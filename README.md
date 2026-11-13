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
| Reported Android security update | November 1, 2023 |
| Installed Android version | Android 12 (confirmed by user in Settings) |
| User-reported kernel | 4.14.190-perf+ (build timestamp: Fri Oct 27 13:02:24 CDT 2023) |
| Project target | Experimental One UI 9 compatibility research |

Android 12 was confirmed by the user in Settings > About phone. The firmware/build, security patch and kernel details are user-reported.

## Current feasibility assessment

See [docs/FEASIBILITY_ASSESSMENT.md](docs/FEASIBILITY_ASSESSMENT.md) for the Android 17 / Moto G60 compatibility review. Treble/VNDK and vendor HAL compatibility remain unverified. The exact Motorola kernel source tag is now identified; see [the kernel-source audit](docs/KERNEL_SOURCE_AUDIT.md).

## One UI 9 research baseline

Samsung confirms that One UI 9 is based on **Android 17**. The target Moto G60 is confirmed to run Android 12, so the project begins with a substantial Android platform gap. See [One UI 9 research](docs/ONEUI9_RESEARCH.md) for official Samsung references and open questions.

No Samsung firmware images or proprietary binaries are stored in this repository. Firmware research is metadata and documentation only.

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

- `docs/` — device information, Samsung firmware research, One UI 9 research, compatibility and porting plan
- `firmware/` — firmware provenance notes and metadata manifests; no firmware images
- `device/hanoip/` — Moto G60 / shared-platform device research
- `kernel/` — Motorola kernel-source references and original configuration research
- `proprietary/` — inventory and licensing notes; no restricted binaries
- `tools/powershell/` — Windows-side project helper documentation and scripts
- `tools/wsl/` — WSL2/Linux research environment documentation
- `research/` — framework, hardware and compatibility investigations

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


## Device-tree audit

The first read-only audit of the historical `hanoip` / SM6150 LineageOS 20 device-tree references is documented in [docs/HANOIP_DEVICE_TREE_AUDIT.md](docs/HANOIP_DEVICE_TREE_AUDIT.md). It records the relevant configuration, firmware identity mismatch, VINTF/HAL and kernel dependencies, and items that must be revalidated. No build or device test was performed.

## Current research milestone

The shared `hanoip` device-tree references have been audited, and the exact Motorola kernel source tag for `S2RIS32.32-20-9-9-2-3` has been identified. The community target index still marks `hanoip` as dropped; vendor/HAL and Android 17 compatibility remain unverified. See [device research](docs/DEVICE_INFORMATION.md), the [device-tree audit](docs/HANOIP_DEVICE_TREE_AUDIT.md), and the [kernel-source audit](docs/KERNEL_SOURCE_AUDIT.md).
