# Samsung Galaxy S26 donor profile

**Project:** OneUI-G60  
**Research target:** Motorola Moto G60 XT2135-2 (hanoip)  
**Research donor:** Samsung Galaxy S26, model `SM-S942B`  
**Research date:** 2026-11-13  
**Status:** Selected for metadata-only research; not validated as a compatible or bootable donor.

## Why this model was selected

No Samsung model with the Moto G60's exact Qualcomm Snapdragon 732G platform was identified during this project's initial donor search. The Galaxy S26 `SM-S942B` was selected because Samsung's official update history documents Android 17 and One UI 9 for this exact model. This is a practical software reference, not a hardware match.

## Official Samsung update metadata

Official Slovak update record:  
https://doc.samsungmobile.com/SM-S942B/037160260310/slk.html

The record lists the following latest build as of 2026-11-13:

| Field | Value |
|---|---|
| Device | Samsung Galaxy S26 |
| Model | `SM-S942B` |
| Latest listed build | `S942BXXS6BZK1` |
| Android version | Android 17 (Samsung record: `C(Android 17)`) |
| Release date | 2026-11-12 |
| Security patch | 2026-11-05 |

The same official model update history identifies One UI 9 in the Android 17 update notes associated with build `S942BXXU4BZI6`. The latest security build listed above is recorded as the current metadata snapshot; availability may vary by CSC, carrier and region.

## Target comparison

| Area | Galaxy S26 donor | Moto G60 target |
|---|---|---|
| Vendor | Samsung | Motorola |
| Model | `SM-S942B` | `XT2135-2` |
| Platform | Different, not a chipset match | Qualcomm Snapdragon 732G / Adreno 618 (project's current device baseline) |
| Android baseline | Android 17 | Android 12, user-confirmed |
| Purpose | Framework/software research reference | Hardware and vendor compatibility target |

The donor and target are different hardware platforms. No compatibility is implied by their Android version relationship.

## Research scope

The initial study may catalogue, under applicable licensing terms, the donor's:
- Android framework and Samsung framework extension version metadata;
- package and partition inventory;
- `system`, `system_ext` and `product` image presence and format;
- framework compatibility matrix and VINTF-related metadata;
- native ABI and declared service dependencies.

The Moto G60's Motorola vendor implementation, device-specific components and kernel remain the target baseline for analysis. Samsung hardware-specific partitions and services are not presumed portable.

## Explicit non-goals and safeguards

- This document does not provide or claim a bootable ROM, flashable image or completed port.
- Do not transplant Samsung `vendor`, `odm`, `boot`, `vendor_boot`, `dtbo`, modem/radio or `vbmeta` to the Moto G60.
- Do not commit firmware images, proprietary binaries, keys or restricted extracted components to this public repository without explicit redistribution permission.
- No flashing, repartitioning, bootloader changes or modification of a device is part of this research milestone.
- Any future compatibility claim requires reproducible evidence for the exact `XT2135-2`.

## Next research milestone

Create a metadata-only inventory of the official donor package and compare the framework-side requirements with the Moto G60's documented partition layout, VINTF declarations, vendor/HAL inventory and kernel baseline. Record unknowns as unknown rather than assuming compatibility.