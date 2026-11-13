# Hanoip device-tree audit — first pass

**Audit date:** 2026-11-13  
**Target:** Motorola Moto G60 XT2135-2, 6 GB RAM  
**Reference branch reviewed:** `lineage-20`  
**Scope:** Read-only source review. No build, device test, firmware extraction or flashing performed.

## Executive summary

The reviewed `motorola-sm6150-devs/device_motorola_hanoip` tree is a useful historical reference for the Moto G40 Fusion / Moto G60 platform. Its product configuration identifies the product as `moto g(60)`, uses the `hanoip` device codename, and is structured around a LineageOS 20-era Android platform.

It is **not** an Android 17 bring-up and is not a ready-made One UI 9 base. The tree depends on a separate SM6150 common device tree, a kernel repository, Motorola hardware support and proprietary vendor files. Each dependency would need its own version and compatibility review.

## Repositories reviewed

- Device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Related Android device tree: https://github.com/motorola-sm6150-devs/android_device_motorola_hanoip/tree/lineage-20
- Shared SM6150 common tree: https://github.com/motorola-sm6150-devs/android_device_motorola_sm6150-common/tree/lineage-20
- Community SM6150 kernel reference: https://github.com/motorola-sm6150-devs/kernel_motorola_sm6150/tree/lineage-20

The second device-tree repository is another historical reference, not evidence of Android 17 support. The review below focuses on the first device tree and the shared common tree it references.

## File-by-file findings

| File / area | What the source says | Initial assessment |
|---|---|---|
| `BoardConfig.mk` | Sets `TARGET_BOARD_PLATFORM := sm6150`, `TARGET_BOOTLOADER_BOARD_NAME := hanoip`, arm64/arm secondary architecture, kernel image and boot-header settings, module aliases, dynamic partition sizes, AVB and SELinux directories. | Valuable hardware/partition/kernel configuration reference. Every boot, partition, AVB and SELinux setting must be revalidated against the exact XT2135-2 firmware and current Android build system before any future prototype. |
| `device.mk` | Inherits Motorola vendor files; enables A/B OTA and virtual A/B configuration; includes fastbootd, boot control, telephony packages and VNDK version 30. | Strong evidence that this is a specific older Android/vendor integration, not a drop-in Android 17 configuration. VNDK 30 and all inherited packages need a compatibility plan. |
| `lineage_hanoip.mk` | Product is `lineage_hanoip`; model is `moto g(60)`; fingerprint is based on Android 12 build `S2RI32.32-20-9-9-2`. | Product identity is relevant, but the fingerprint does not match the user's reported string `S2RIS32.32-20-9-9-2-3`. Treat firmware provenance as unconfirmed until exact properties and vendor files are compared. |
| `lineage.dependencies` | Pulls `android_hardware_motorola`. | Dependency must be pinned to a known revision and checked for current API/build compatibility. |
| `proprietary-files.txt` | Lists Motorola/Qualcomm libraries, frameworks, apps and services; comments say most entries originate from `S2RI32.32-20-9-9-2`. | Inventory is useful for dependency mapping only. Do not copy or publish proprietary binaries without redistribution permission. Exact source-build match is not established. |
| `modules.load` | Lists audio, WLAN, fingerprint, touchscreen, sensor and Motorola kernel modules. | Useful checklist for matching kernel source, module names, firmware and vendor expectations. It does not prove the modules build or load on the user's exact firmware. |
| `system.prop`, `product.prop`, `system_ext.prop` | Contain Qualcomm/Motorola audio, radio, media, graphics and other device properties. | Treat as legacy tuning data, not values to transplant. Each property needs ownership, version and runtime validation on the target platform. |
| Shared `BoardConfigCommon.mk` | Defines SM6150 common build settings and references a kernel under `kernel/motorola/sm6150`; it also contains a GPU platform setting. | Shared configuration is not automatically exact-SKU truth. In particular, the common-tree GPU label should be checked against the actual device GPU and the effective target configuration. |
| Shared `lineage.dependencies` | References `android_kernel_motorola_sm6150` and `android_hardware_motorola`. | Kernel and Motorola hardware repositories are required parts of the source graph; the device tree alone is incomplete. |
| Shared `manifest.xml` | Declares a device VINTF manifest with target-level 4 and multiple HIDL HALs, including audio, camera, radio, boot, Bluetooth and other services. | A key compatibility artifact. It must be compared with the intended Android 17 framework compatibility matrix and actual vendor HAL implementations; the file alone does not establish compatibility. |
| Shared `compatibility_matrix.xml` | Declares required and optional framework-facing HAL interfaces. | Must be reviewed as a pair with the device manifest and the exact vendor image. |
| Shared `rootdir/etc/fstab.qcom` | Contains mount definitions for system, userdata, modem firmware, DSP, persist, Bluetooth firmware, FSG and misc. | Historical partition/mount reference only. Confirm against the exact XT2135-2 partition map and current fstab/first-stage init requirements. |
| Shared SELinux rules | Include vendor file contexts and device-specific permissions for audio, radio, camera, fingerprint and thermal paths. | Useful policy inventory, but Android 17 policy compatibility and least-privilege review are outstanding. Never solve denials by broadly weakening policy. |

## Important discrepancies and unknowns

1. **Firmware identity:** the tree's fingerprint and proprietary-file comments refer to `S2RI32.32-20-9-9-2`; the user-reported installed build is `S2RIS32.32-20-9-9-2-3`. These strings are not identical. We do not yet know whether the tree's proprietary-file list matches the exact installed vendor image.
2. **GPU configuration:** the shared SM6150 common configuration contains a GPU platform label that should be checked against the Moto G60's reported Adreno 618 hardware and the final effective build variables. This is a verification item, not a confirmed defect.
3. **Kernel provenance:** the device tree points to `kernel/motorola/sm6150` and supplies a device-specific defconfig list. The user's reported runtime kernel is `4.14.190-perf+`; the exact source commit, defconfig and patches corresponding to build `S2RIS32.32-20-9-9-2-3` remain unmatched.
4. **VINTF/HAL age:** the shared manifest uses HIDL interfaces and target-level 4. The exact compatibility requirements for the intended Android 17 framework have not been compared yet.
5. **Partition assumptions:** dynamic partitions, A/B/virtual A/B, boot header version, AVB and recovery fstab assumptions need confirmation against the exact device's partition metadata and boot chain.
6. **SKU identity:** the public trees describe G40 Fusion/G60 under the shared `hanoip` codename. That does not independently prove every file or every hardware peripheral is identical on XT2135-2.

## What appears reusable as research

- Device codename and SoC-family build identifiers.
- Historical boot image, DTBO, partition and AVB configuration as items to verify.
- Kernel defconfig names and module inventory as a source-matching checklist.
- VINTF manifest and compatibility matrix as starting points for interface mapping.
- HAL, property and SELinux inventories as a map of hardware dependencies.
- Proprietary-file manifest as a list of component names and provenance claims, not as permission to redistribute the files.

## What must not be treated as reusable without rework

- The Android 12 / LineageOS 20 product makefiles and VNDK 30 settings.
- The old build fingerprint or assumed firmware provenance.
- The kernel configuration and module list without matching them to the exact kernel source.
- Vendor properties, HAL versions, VINTF declarations and SELinux rules.
- Partition sizes, boot header, AVB flags, fstab and OTA settings.
- Any proprietary blob or Samsung component without a separate compatibility and licensing review.

## Recommended next research gate

The next useful step is a **source provenance and interface comparison**, not a build:

1. Match the exact Motorola kernel source revision and configuration to the user's reported firmware/kernel.
2. Compare the tree's expected vendor source build with the actual firmware build identity, without uploading proprietary images or blobs.
3. Map the device and framework VINTF declarations and identify which HALs are HIDL/AIDL and which versions are required.
4. Verify the effective GPU target setting and the actual GPU userspace/kernel driver pairing.
5. Record the exact partition/boot metadata from trusted device documentation or a later read-only inventory.

No device modification is needed for this research milestone. No ROM, boot image or working feature has been produced or tested.

## References

- Device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Shared SM6150 common tree: https://github.com/motorola-sm6150-devs/android_device_motorola_sm6150-common/tree/lineage-20
- Related Android device tree: https://github.com/motorola-sm6150-devs/android_device_motorola_hanoip/tree/lineage-20
- SM6150 kernel reference: https://github.com/motorola-sm6150-devs/kernel_motorola_sm6150/tree/lineage-20
- LineageOS SM6150 common device documentation: https://github.com/LineageOS/android_device_motorola_sm6150-common
- TeamWin hanoip device specifications (community reference): https://github.com/TeamWin/android_device_motorola_hanoip
