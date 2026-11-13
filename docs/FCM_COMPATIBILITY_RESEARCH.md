# Framework Compatibility Matrix (FCM) research note

**Project:** OneUI-G60  
**Target:** Motorola Moto G60 XT2135-2, user-confirmed Android 12  
**Research donor:** Samsung Galaxy S26 SM-S942B, selected as Android 17 / One UI 9 software reference  
**Research date:** 2026-11-13  
**Status:** Source-level research only; target-device VINTF data has not been collected.

## What FCM means

The Framework Compatibility Matrix describes the HAL and interface requirements a framework expects from the device/vendor implementation. The device manifest describes the HAL services provided by the device. Android's VINTF matching process reconciles the framework compatibility matrix with the device manifest, and the framework manifest with the device compatibility matrix.

Official references:
- FCM lifecycle: https://source.android.com/docs/core/architecture/vintf/fcm
- VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- Compatibility matrix format: https://source.android.com/docs/core/architecture/vintf/comp-matrices

## Known Android 12 baseline

AOSP documents Android 12 as normally corresponding to FCM level `6`. A device shipping with Android 12 generally has FCM level 6, although a device may implement a newer level. This is a general expectation, not proof of the exact XT2135-2 manifest's `target-level`.

| Item | Current project status |
|---|---|
| Installed Android release | Android 12, user-confirmed |
| Expected FCM for Android 12 | Level `6`, according to AOSP general guidance |
| Exact device manifest `target-level` | Not collected from XT2135-2 |
| Exact device compatibility matrix | Not collected |
| HAL names, versions and instances | Not inventoried from target device |

## Donor-side caution

The current AOSP FCM lifecycle documentation lists Android 12 through Android 16 FCM mappings, but does not yet provide an Android 17 FCM mapping in that table. Therefore this project must not invent or assume an Android 17 FCM number. When an authoritative Android 17 FCM mapping and source matrix become available, record the actual level and inspect the matching requirements.

Even when a framework supports older FCM levels, the exact device manifest and required HAL versions still need to satisfy the relevant compatibility matrix. FCM level arithmetic alone cannot establish compatibility.

## What this means for OneUI-G60

The Moto G60's Android 12 baseline is a starting point for identifying its vendor interface generation. It is not enough to decide whether an Android 17 framework can work. The next evidence must come from the exact XT2135-2:

1. Device VINTF manifest and its `target-level`.
2. Device compatibility matrix and any product/ODM VINTF fragments.
3. HAL service names, transport, versions and instances.
4. Kernel compatibility requirements declared in framework matrices.
5. Vendor API / VNDK metadata and linker namespace configuration.

Only after those records are available can the team compare the target vendor implementation against the donor framework requirements. Samsung's One UI framework may also require proprietary Samsung services and APIs beyond ordinary AOSP VINTF compatibility.

## Current conclusion

FCM is a useful compatibility framework for this project, but no target-device VINTF evidence has been collected yet. The gate remains **unknown / pending**. This note does not claim that the Moto G60 can run Android 17 or One UI 9.

## Public hanoip device-tree evidence (historical, not stock-device proof)

A public community device tree was inspected:
- Repository: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- Branch inspected: `lineage-20`
- Its README identifies the tree as Motorola Moto G40 Fusion / Moto G60 (`hanoip`).
- `BoardConfig.mk` declares `TARGET_BOARD_PLATFORM := sm6150` and `TARGET_BOOTLOADER_BOARD_NAME := hanoip`.
- `device.mk` declares `BOARD_SHIPPING_API_LEVEL := 30`, `PRODUCT_SHIPPING_API_LEVEL := 30`, `PRODUCT_TARGET_VNDK_VERSION := 30`, and `PRODUCT_EXTRA_VNDK_VERSIONS := 30`.

Important limitations:
- This is a community LineageOS 20-era build tree, not Motorola's original Android 12 stock source tree and not a dump from the user's exact XT2135-2.
- The inspected device-tree file inventory did not contain a VINTF manifest or compatibility-matrix XML. The build inherits proprietary vendor makefiles, so the missing VINTF declarations may be supplied by separate vendor repositories or extracted vendor files.
- The tree's shipping API/VNDK values are build configuration for that community target. They must not be treated as proof of the stock XT2135-2 manifest target level, installed vendor API, or actual runtime VNDK.
- The tree's shared Moto G40 Fusion/G60 naming does not by itself prove every file and HAL configuration is identical for every regional SKU.

This gives us a useful historical reference for the hanoip software stack, but it does not complete either VINTF matching pair.

## Preliminary VINTF audit matrix

| Check | Evidence found | Status |
|---|---|---|
| Exact XT2135-2 device manifest | Not yet obtained | **Pending** |
| Device manifest `target-level` | Not yet obtained | **Pending** |
| Vendor and ODM HAL declarations | Not present in inspected public device-tree inventory | **Pending** |
| Device compatibility matrix | Not yet obtained | **Pending** |
| Community hanoip build shipping API | API 30 declared in LineageOS 20 tree | Historical reference only |
| Community hanoip build VNDK | Version 30 declared in LineageOS 20 tree | Historical reference only |
| Android 17 framework FCM level and matrix | No authoritative Android 17 level recorded in this audit | **Pending** |
| Samsung One UI proprietary framework requirements | Not inventoried | **Pending** |

## Next evidence to collect from the target

When the PC is available, collect read-only VINTF/runtime metadata from the phone and compare it with the public source evidence. The key items are the device manifest's `target-level`, vendor and ODM manifest fragments, device compatibility matrix, HAL interface versions/instances, and actual VNDK/linker configuration. Redact serial numbers, IMEI, account identifiers, and other personal identifiers from any logs before sharing them.

No flashing, partition changes, or firmware modification is part of this audit.


## Android 17 framework requirements for an FCM 6 device

AOSP's Android 17 build configuration in the `android16-qpr2-release` source branch explicitly includes `framework_compatibility_matrix.6.android17.xml` in the Android 17 system matrix set. This is an Android 17-specific compatibility matrix for devices targeting FCM 6; it is distinct from the ordinary Android 12-era `compatibility_matrix.6.xml`.

Official source files:
- Android 17 matrix build selection: https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/Android.bp
- Android 17 FCM 6 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/compatibility_matrix.6.android17.xml

The inspected Android 17 FCM 6 matrix contains these radio HAL requirements:

| HAL | Interface | Version | Instances |
|---|---|---:|---|
| `android.hardware.radio` (HIDL) | `IRadio` | 1.6 | `slot1`, `slot2`, `slot3` |
| `android.hardware.radio` (HIDL) | `ISap` | 1.2 | `slot1`, `slot2` |
| `android.hardware.radio.config` (HIDL) | `IRadioConfig` | 1.1 | `default` |
| `android.hardware.radio.config` (HIDL) | `IRadioConfig` | 1.3 | `default` |

The matrix comments that Android 17 provides special extended support for `IRadio` on devices launching with FCM level 6. This is a meaningful compatibility accommodation for older devices, but it is not proof that a particular device passes VINTF matching. We still need the exact XT2135-2 device manifest and its radio HAL declarations to check versions and instances.

The same Android 17 build file lists the supported framework matrix set for Android 17 devices as FCM 6 (using the Android-17-specific file), 7, 8, 202404, 202504, and 202604. It lists a separate older set for Android 16 QPR builds. Do not describe 202604 as the required FCM level for an Android 12/FCM 6 device: the special FCM 6 matrix is the relevant compatibility path to inspect for this target.

### What this tells us about Moto G60

- If the stock XT2135-2 manifest targets FCM 6, the Android 17 FCM 6 radio matrix is directly relevant.
- We must verify whether the target exposes the required radio and radio-config HAL versions and instance names.
- The public community hanoip tree inspected earlier does not include the stock vendor manifest, so it cannot answer this question.
- This matrix is only one piece of the Android 17 framework contract. Other installed framework matrices, product/system_ext requirements, kernel requirements, SELinux policy, VNDK/linker namespaces, and Samsung proprietary framework dependencies remain separate checks.
- No compatibility verdict can be made until the target manifest and compatibility matrix are collected and checked.

**Research status:** Android 17 FCM 6 radio requirements identified from AOSP source; target-side match remains pending.
