# Moto G60 Treble / VNDK compatibility audit

**Project:** OneUI-G60  
**Target:** Motorola Moto G60 XT2135-2 (hanoip), Android 12 baseline  
**Research date:** 2026-11-13  
**Status:** Pending read-only evidence from the exact target phone. No values are assumed.

## Purpose

Before evaluating Samsung Galaxy S26 One UI 9 framework components, establish what the Moto G60's existing Android 12 vendor implementation exposes and whether Android's documented cross-version system/vendor boundary is present.

This is a read-only information-gathering milestone. It is not a GSI installation, One UI port, build, or device-modification procedure.

## Why this gate comes first

Android's GSI documentation identifies these checks when assessing whether a device can support a newer generic Android system image:

1. Treble support: `ro.treble.enabled` must report `true`.
2. Cross-version VNDK support: inspect the `[vendor]` namespace in the applicable `/system/etc/ld.config*.txt` file; `namespace.default.isolated=true` indicates the documented cross-version VNDK condition.
3. CPU ABI: record `ro.product.cpu.abi` and confirm the architecture expected by the system image.

Official reference: https://developer.android.com/topic/generic-system-image

AOSP VINTF documentation explains that device manifests describe services provided by the vendor implementation, while framework compatibility matrices describe what the framework expects. The manifest/matrix pairs must match; see https://source.android.com/docs/core/architecture/vintf and https://source.android.com/docs/core/architecture/vintf/match-rules.

**Important limitation:** passing these checks would only provide evidence about generic Android framework/vendor compatibility. It would not prove that Samsung One UI 9, Samsung SystemUI, Samsung services, or Samsung applications can run on the Moto G60.

## Read-only evidence checklist

If Android Debug Bridge (ADB) is already available on a trusted computer and the phone is already authorized for that computer, the following queries are informational/read-only. Do not unlock the bootloader, flash images, change partitions, or install anything for this audit.

### 1. Basic platform properties

```sh
adb shell getprop ro.treble.enabled
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abilist
adb shell getprop ro.build.version.release
adb shell getprop ro.build.version.sdk
adb shell getprop ro.vndk.version
adb shell getprop ro.board.platform
```

Record the returned values exactly. A blank property means unknown/not exposed by that property; do not infer a value.

### 2. Vendor linker namespace configuration

Android 11 and later generate linker configuration at runtime under `/linkerconfig`. The AOSP linker-namespace documentation identifies `/linkerconfig/ld.config.txt` as the runtime configuration. The GSI compatibility guide describes checking the applicable `/system/etc/ld.config.<version_identifier>.txt` file. For this Android 12 target, record which path exists and inspect the runtime file first:

```sh
adb shell ls -l /linkerconfig/ld.config.txt
adb shell ls /system/etc/ld.config*.txt
adb shell grep -A 20 '\[vendor\]' /linkerconfig/ld.config.txt
```

If the runtime file is unavailable, record that fact and inspect any applicable `/system/etc/ld.config*.txt` file without changing it. In the `[vendor]` section, record the value of `namespace.default.isolated` exactly. Google's GSI guide says `true` indicates full VNDK support for newer-version GSI use; `false` limits the documented GSI path to the same Android OS version. This is a GSI eligibility criterion, not a guarantee of Samsung framework compatibility. Do not edit linker configuration files.

### 3. VINTF inventory

List the VINTF manifest locations without copying full device logs:

```sh
adb shell ls /vendor/etc/vintf/
adb shell ls /vendor/etc/vintf/manifest/
```

If the listed paths differ or a directory is absent, record that fact. Do not assume missing output means the device has no HALs; manifests can be split across files and locations.

## Evidence table

| Check | Required record | Current status |
|---|---|---|
| `ro.treble.enabled` | Exact returned value | Not collected |
| `ro.product.cpu.abi` | Exact returned value | Not collected |
| `ro.product.cpu.abilist` | Exact returned value | Not collected |
| Android release / SDK | Exact returned values | Android 12 confirmed; SDK not collected |
| `ro.vndk.version` | Exact returned value or blank | Not collected |
| Vendor linker config | Runtime `/linkerconfig/ld.config.txt` (or applicable static config), `[vendor]` section and exact `namespace.default.isolated` value | Not collected |
| VINTF directory inventory | Existing paths / manifest filenames | Not collected |
| HAL compatibility | Compare manifests and matrices after lawful metadata collection | Not assessed |

## Privacy and handling

- Do not share IMEI, serial number, phone number, account names, Wi-Fi identifiers, or unredacted bug reports/log dumps.
- Share only the specific property values and filenames requested in this document.
- Do not publish full proprietary manifests or extracted firmware files; record only the minimum metadata needed for compatibility analysis and review redistribution rights first.
- If ADB is not already set up, this milestone can wait. Do not change phone settings or install tools just to rush the audit.

## Decision criteria

| Result | Interpretation |
|---|---|
| Treble is `false` | The official GSI path is not supported; stop treating GSI compatibility as a likely route. |
| Treble is `true`, VNDK namespace isolation is `true` | Positive evidence for the documented cross-version GSI condition, not proof of One UI compatibility. |
| Treble is `true`, VNDK namespace isolation is `false` | The official GSI guidance limits the device to a GSI matching its on-device Android version; this is a major obstacle to an Android 17 system/vendor pairing. |
| Values are missing or ambiguous | Keep the gate unresolved; collect better evidence rather than guessing. |

Even a positive result leaves VINTF, HAL versions, SELinux, graphics, media, audio, camera, radio, kernel interfaces, Samsung framework dependencies, partition capacity and licensing to be assessed separately.

## Current conclusion

No Treble, VNDK, ABI or VINTF values have yet been collected from the exact XT2135-2. The compatibility gate is therefore **unknown / pending**. No image should be modified or tested on the phone based on this document.