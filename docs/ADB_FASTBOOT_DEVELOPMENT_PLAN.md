# ADB / Fastboot Development Plan

> **Project status: PRE-RELEASE — IN DEVELOPMENT**
>
> **No OneUI-G60 image exists yet. No bootable or installable artifact is produced by this document.**

## Purpose

Track the host-side Android Debug Bridge (ADB) and Fastboot tooling that may eventually support controlled development and validation of OneUI-G60 for the Motorola Moto G60 (XT2135-2).

This is a planning document, not an installation guide. The current project remains in research and feasibility assessment.

## Planned tooling areas

- ADB tool availability and version identification.
- Fastboot tool availability and version identification.
- Read-only device identification and connection diagnostics.
- Read-only collection of build, product, and VINTF metadata where available.
- Build-artifact naming, provenance, checksums, and validation records.
- Clear separation between bootloader Fastboot and userspace Fastboot (fastbootd).

## Not implemented

- No ADB/Fastboot automation scripts.
- No bootloader unlocking or relocking workflow.
- No partition write, erase, format, or flashing workflow.
- No image booting workflow.
- No device-side changes.
- No OneUI-G60 boot, recovery, vendor, system, or super image.

## Image artifact policy

Do not create an empty, dummy, or arbitrary file with an `.img` extension and present it as a development image. An Android image must be generated from a defined build target and validated against the intended device layout and boot-image format.

When a genuine artifact eventually exists, record at minimum:

- Exact artifact type and intended partition or purpose.
- Build target, source revision, and build date.
- Image format/header validation results.
- File size and SHA-256 checksum.
- Compatibility status and known limitations.
- Explicit release status.

Until those checks are complete, the repository must not publish an image as bootable, flashable, or test-ready.

## Current status

| Component | Status |
|---|---|
| ADB / Fastboot research | Planned |
| Read-only diagnostics | Planned |
| Device-specific boot image definition | Not established |
| Android 17 framework / VINTF compatibility | Under research |
| Wi-Fi adaptation | Planned research |
| Audio adaptation | Planned research |
| OneUI-G60 image build | Not started |
| Bootability validation | Not started |

## Safety and compatibility notes

ADB and Fastboot are general Android development tools, but some operations can erase user data or alter device partitions. This project does not currently authorize or document those operations. In particular, bootloader unlocking can trigger a factory reset, and flashing an incompatible image can make a device unusable.

References:
- [AOSP: Lock and unlock the bootloader](https://source.android.com/docs/core/architecture/bootloader/locking_unlocking)
- [AOSP: Fastbootd](https://source.android.com/docs/core/architecture/bootloader/fastbootd)
- [AOSP: Bootloader overview](https://source.android.com/docs/core/architecture/bootloader)
