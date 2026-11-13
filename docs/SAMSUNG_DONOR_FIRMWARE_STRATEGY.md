# Samsung One UI 9 donor-firmware strategy

**Project:** OneUI-G60  
**Target:** Motorola Moto G60 XT2135-2 (hanoip), Android 12 baseline  
**Research date:** 2026-11-13  
**Status:** Planning only. No Samsung firmware images have been acquired, extracted, modified, or tested.

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

**Not selected yet.** Do not download, unpack, or publish a donor package until the exact donor model and build are recorded and its firmware provenance and redistribution terms are understood.

The donor should be an official One UI 9 / Android 17 build, with a clearly documented model and build identity. Similar CPU architecture alone does not make its vendor image compatible with the Moto G60.

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

The correct next input is the **exact Samsung donor model and firmware build**. Until a donor is selected, there is no responsible way to identify the relevant One UI 9 `.img` files or assess their compatibility.

The first engineering comparison after donor selection should be the donor image inventory against the Moto G60's existing partition map and VINTF/vendor interface—not replacing Motorola's vendor image with Samsung's.
