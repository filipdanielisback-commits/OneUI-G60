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