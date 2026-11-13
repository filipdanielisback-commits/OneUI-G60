# Compatibility tracking

See [the Android 17 / One UI 9 feasibility assessment](FEASIBILITY_ASSESSMENT.md) for the current platform-gap analysis and research gates.

The target is Android 12 with a user-reported 4.14.190-perf+ kernel; One UI 9 is based on Android 17. Treble/VNDK and exact vendor compatibility have not yet been checked. The kernel version comparison is a warning, not a final compatibility verdict.

This is a planning matrix, not a claim of current functionality.

| Area | Target | Status | Evidence / notes |
|---|---|---|---|
| Android framework | Android 17 userspace against Moto G60 Android 12 baseline | Research needed | Five-generation platform gap; see feasibility assessment |
| Samsung framework | Identify lawful, version-matched dependencies | Research needed | |
| SystemUI | Assess framework/API and service dependencies | Research needed | |
| Vendor / HAL | Verify Moto G60 HALs, VINTF, Treble and cross-version VNDK | Research needed | No on-device properties or VINTF evidence collected yet |
| Kernel | Verify exact XT2135-2 kernel source/configuration and newer userspace support | Research needed | Reported 4.14.190-perf+; outside documented Android 17 GKI kernel matrix, but GKI matrix is not a direct verdict for this legacy non-GKI device kernel |
| Wi-Fi | Validate driver, firmware provenance and HAL | Not tested | |
| Battery | Validate power supply and health interfaces | Not tested | |
| Audio | Validate audio HAL, routing and codecs | Not tested | |
| GPU | Validate userspace/kernel driver compatibility | Not tested | |
| CPU | Validate scheduler, thermal and frequency interfaces | Not tested | |
| Apps | Test boot, install and runtime compatibility | Not tested | |
| Bluetooth | Future development target | Not tested | |
| Camera | Validate camera provider and sensor stack | Not tested | |
| Sensors | Validate sensor HAL and individual sensors | Not tested | |
| Cellular | Validate modem/RIL and emergency calling | Not tested | |
| Fingerprint | Validate sensor and biometric integration | Not tested | |

## Status definitions

- **Research needed:** No implementation or test evidence yet.
- **In development:** Work is underway but not validated.
- **Working:** Reproducibly tested on the exact XT2135-2 target, with test steps and evidence recorded.
- **Broken:** Tested and known not to work.
- **Not tested:** No reliable test has been completed.

A feature must not be marked Working based only on a successful build or boot.
