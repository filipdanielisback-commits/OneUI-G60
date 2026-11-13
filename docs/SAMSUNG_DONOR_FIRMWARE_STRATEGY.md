# Samsung One UI 9 donor-firmware strategy

**Project:** OneUI-G60  
**Target:** Motorola Moto G60 XT2135-2 (hanoip), Android 12 baseline  
**Research date:** 2026-11-13  
**Status:** Donor selected for read-only research. No Samsung firmware images have been acquired, extracted, modified, or tested.

## Objective

Investigate a genuine cross-vendor One UI 9 port for the Moto G60. The intended result is Samsung's real One UI framework and Samsung system applications adapted to the Motorola target—not an AOSP ROM with a Samsung-themed launcher.

A donor firmware is a source of framework-side components to study. It is not a complete firmware package that can be installed on the Moto G60.

## Critical system/vendor distinction

Android separates the generic operating-system framework from hardware-specific implementation:

- `system`: core Android platform and OEM framework components.
- `system_ext`: OEM extensions tightly coupled to system framework.
- `product`: product-specific applications, resources, permissions and configuration.
- `vendor`: SoC/device hardware implementation, native libraries, HALs, firmware-facing services and configuration.
- `odm`: board/SKU-specific components such as HALs, modules, init files, policy and configuration.
- `boot`, `vendor_boot`, `dtbo`: kernel, ramdisk and hardware/boot-chain-specific material, depending on device generation and layout.
- `vbmeta`: verified-boot metadata and integrity relationships.

AOSP documentation describes `vendor` and `odm` as hardware-specific images and VINTF as the interface between system-side and vendor-side components. See:
- https://source.android.com/docs/core/architecture/partitions
- https://source.android.com/docs/core/architecture/partitions/shared-system-image
- https://source.android.com/docs/core/architecture/partitions/odm-partitions

## Initial porting hypothesis

For a cross-vendor Moto G60 target, the starting hypothesis is:

| Image / component | Initial treatment | Reason |
|---|---|---|
| Samsung `system` | Donor research candidate | Contains Android framework and Samsung framework-side components |
| Samsung `system_ext` | Donor research candidate | Often contains OEM framework extensions tightly coupled to system |
| Samsung `product` | Donor research candidate | May contain One UI product apps, overlays and configuration |
| Motorola `vendor` | Preserve as target baseline | Contains Moto G60 / Qualcomm hardware implementation |
| Motorola `odm` (if present) | Preserve as target baseline | Board/SKU-specific implementation |
| Motorola kernel / boot chain | Preserve as target baseline for research | Must match Moto G60 hardware and boot architecture |
| Samsung `vendor`, `odm`, `boot`, `vendor_boot`, `dtbo`, modem/radio | Do not transplant | Built for different hardware and boot chain |
| Samsung `vbmeta` | Do not transplant | Verified-boot metadata is tied to donor images and signing/rollback configuration |

This is a hypothesis, not a recipe or guarantee. Individual Samsung components may depend on Samsung vendor HALs, services, kernel drivers, security architecture, proprietary libraries or framework APIs that do not exist on Motorola. Some components may need replacement, adaptation or may prove non-portable.

## Why a donor model is required

One UI 9 firmware must be tied to an exact Samsung model, region/CSC, Android base version and build. The donor model determines:
- which One UI 9 framework build is being studied;
- CPU architecture and native library ABI;
- Samsung services and HAL dependencies;
- VINTF declarations and vendor interface versions;
- partition layout and where the images are stored;
- which features are specific to donor hardware.

There is no valid generic “One UI 9 system.img” that is guaranteed to work across Samsung devices. A donor's `vendor.img` is not a substitute for the Moto G60's vendor implementation.

## Donor selection status

**Selected research donor: Samsung Galaxy S26, model `SM-S942B` (European model).** This is a documentation/research selection, not a compatibility finding or a claim that its firmware can boot on the Moto G60.

Samsung's official Slovak software-update record for `SM-S942B` lists the following current build information as of 2026-11-13:

| Field | Recorded value |
|---|---|
| Donor device | Galaxy S26 |
| Model | `SM-S942B` |
| Official update record | https://doc.samsungmobile.com/SM-S942B/037160260310/slk.html |
| Latest listed build | `S942BXXS6BZK1` |
| Android version | Android 17 (shown by Samsung as `C(Android 17)`) |
| Release date | 2026-11-12 |
| Security patch level | 2026-11-05 |
| One UI 9 evidence | Samsung's same model update history identifies One UI 9 in the Android 17 update notes (for build `S942BXXU4BZI6`). |

The latest listed build above is the current metadata snapshot, not a claim that every CSC/carrier has received it. Re-check the official record before any future analysis because firmware availability and build numbers can change.

The Galaxy S26 is not a chipset match for the Moto G60. It is selected because it provides a documented official Android 17 / One UI 9 system-software reference. Its hardware-specific images and services are not presumed compatible. Do not treat Samsung `vendor`, `odm`, `boot`, `vendor_boot`, `dtbo`, modem/radio or `vbmeta` as target components.

## Evidence required before any port work

1. Exact donor model, region/CSC, build number and Android base version.
2. Official firmware provenance and package manifest; record filenames and hashes as metadata only.
3. Image inventory: identify whether `super.img` contains logical `system`, `system_ext`, `product`, `vendor` and/or `odm` images, and record actual image formats.
4. Moto G60 target partition map and dynamic-partition metadata from trustworthy existing project records.
5. Donor and target VINTF manifests and framework compatibility matrices.
6. HAL inventory and dependencies, including HIDL/AIDL versions and required vendor services.
7. ABI and native-library compatibility, SELinux policy, init services, permissions, overlays and framework resource dependencies.
8. Space and partition-size feasibility for the target layout.
9. A licensing review before any binary is stored or redistributed.

## Legal and repository rules

- Do not commit Samsung or Motorola firmware images, proprietary binaries, vendor blobs, modem files, keys or extracted restricted components to this public repository unless redistribution is explicitly permitted.
- Keep source links, build identifiers, image filenames, hashes, manifests and original analysis notes where lawful.
- Do not imply Samsung endorsement. OneUI-G60 is an independent research project.
- Do not claim a real port, boot, or working feature until there is reproducible evidence on the exact XT2135-2.
- No flashing, repartitioning or bootloader changes are part of this research stage.

## Current decision

The donor identity is now recorded as **Galaxy S26 `SM-S942B`**, with `S942BXXS6BZK1` as the latest build listed on Samsung's Slovak update page on 2026-11-13. The next research task is a metadata-only comparison: document the Samsung firmware package/image inventory and compare its Android framework requirements with the Moto G60's known partition map, VINTF declarations and vendor/HAL baseline. No binary redistribution, flashing, repartitioning or device modification is part of this milestone.
