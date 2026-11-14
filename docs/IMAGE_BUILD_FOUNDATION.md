# OneUI-G60 Image Build Foundation

> **PRE-RELEASE — IN DEVELOPMENT**
>
> No OneUI-G60 image has been built. This document records prerequisites and validation gates only. It is not an installation or flashing guide.

## Objective

Establish the evidence and source configuration required before a legitimate device-specific Android image can be produced for the Motorola Moto G60 (XT2135-2).

## Known target baseline

- Device: Motorola Moto G60
- Exact model: XT2135-2
- Known stock Android baseline: Android 12
- Known kernel baseline: Linux 4.14.190-perf+
- Kernel source tag recorded in project research: MMI-S2RIS32.32-20-9-9-2-3
- SoC / board identification: Snapdragon 732G / hanoip is a research lead; exact stock SKU mapping remains to be verified.

## Required inputs — not yet fully verified

| Input | Current state | Required evidence |
|---|---|---|
| Device tree / product configuration | Incomplete | Exact XT2135-2-specific build configuration |
| Kernel source and configuration | Source identified; integration unproven | Reproducible build configuration and compatibility review |
| Device manifest and VINTF fragments | Pending device-side audit | Stock manifest, fragments, target-level and device matrix |
| Partition and image layout | Unknown | Authoritative device-specific layout metadata |
| Boot image format | Unknown for this target build | Stock image header metadata and corresponding board configuration |
| Vendor ramdisk / DTB arrangement | Unknown | Verified stock image metadata and device configuration |
| Android 17 framework compatibility | Under research | FCM/VINTF comparison and HAL compatibility evidence |
| Wi-Fi / Audio / Camera / Bluetooth | Planned research | HAL, driver, firmware and framework compatibility audits |
| Proprietary vendor components | Not collected | Rights-aware inventory; no redistribution without permission |
| Build environment | Not established | Reproducible host environment and source manifest |

## Build artifact stages

1. **Research-only:** collect and document public source and device metadata.
2. **Configuration stage:** define a device-specific product configuration after verifying exact hardware and partition facts.
3. **Source integration:** assess kernel, vendor interfaces, framework compatibility and required device components.
4. **Compile stage:** produce candidate artifacts from a reproducible source revision.
5. **Static validation:** verify image format, metadata, checksums, partition-size constraints and build provenance.
6. **Compatibility review:** document unresolved requirements and independent review results.

No stage should be marked complete without recorded evidence.

## Boot image format is not guessed

AOSP documents multiple boot image header versions and separate vendor_boot layouts. The correct format depends on the target device's architecture and launch configuration; a header version must not be selected merely because it is newer.

References:
- [AOSP boot image header](https://source.android.com/docs/core/architecture/bootloader/boot-image-header)
- [AOSP vendor boot partitions](https://source.android.com/docs/core/architecture/partitions/vendor-boot-partitions)

## Artifact naming and release policy

A future candidate artifact must identify its actual purpose, for example a boot image versus a vendor boot image. The project must not publish a generic `OneUI-G60.img` until its partition role and format are known.

Every real artifact must have:
- Source revision and build identifier.
- Exact artifact type and intended device/partition role.
- File size and SHA-256 checksum.
- Image-header and structural validation results.
- Known limitations and test status.
- Clear **UNTESTED**, **NON-BOOTABLE**, or **BOOTABILITY NOT VERIFIED** status as applicable.

A placeholder document is not an image. An arbitrary or empty binary is not a build artifact.

## Current build status

- Device-specific build configuration: **NOT ESTABLISHED**
- Android 17 source integration: **NOT STARTED**
- Image compilation: **NOT STARTED**
- Image validation: **NOT STARTED**
- Bootability: **NOT ESTABLISHED**
- Release channel: **PRE-RELEASE / IN DEVELOPMENT**

## Scope and safety

This project phase is documentation and compatibility research. It does not include bootloader changes, partition writes, flashing, or instructions to attempt booting an unverified image. Do not include personal identifiers in diagnostic material. Do not upload proprietary firmware or vendor binaries unless redistribution rights are explicit.
