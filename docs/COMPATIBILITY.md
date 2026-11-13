# Compatibility tracking

This is a planning matrix, not a claim of current functionality.

| Area | Target | Status | Evidence / notes |
|---|---|---|---|
| Android framework | Determine compatible Android base and framework dependencies | Research needed | |
| Samsung framework | Identify lawful, version-matched dependencies | Research needed | |
| SystemUI | Assess framework/API and service dependencies | Research needed | |
| Vendor / HAL | Verify Moto G60 HALs and VINTF | Research needed | |
| Kernel | Verify exact XT2135-2 kernel source/configuration | Research needed | |
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
