# Exact Motorola kernel-source audit

**Audit date:** 2026-11-13  
**Target:** Moto G60 XT2135-2, firmware `S2RIS32.32-20-9-9-2-3`  
**Method:** Read-only inspection of Motorola's public source tag and community device/kernel configuration. No kernel build or device test was performed.

## Executive result

The exact Motorola kernel source tag for the user's reported firmware has been identified:

- Tag: `MMI-S2RIS32.32-20-9-9-2-3`
- Official source: https://github.com/MotorolaMobilityLLC/kernel-msm
- Tag page: https://github.com/MotorolaMobilityLLC/kernel-msm/releases/tag/MMI-S2RIS32.32-20-9-9-2-3
- Motorola publication record: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Tag page short commit identifier: `f6673a1`
- Motorola's tag release note: “Hanoip push for Android 12”

Issue #574 names the exact firmware package:
`XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3_subsidy-DEFAULT_regulatory-XT2135-2-EUROPE_CFC.xml.zip`.
A Motorola maintainer linked the exact tag and closed the issue as completed on December 7, 2023.

**This resolves the source-tag discovery gap.** It does not prove that the source compiles with a modern toolchain, that the published tree exactly reproduces the installed binary, or that this legacy kernel can support Android 17 / One UI 9.

## Verified kernel version

The official tag's top-level `Makefile` declares:

- `VERSION = 4`
- `PATCHLEVEL = 14`
- `SUBLEVEL = 190`
- `CONFIG_LOCALVERSION="-perf"` in the main performance defconfig

This matches the version family in the user's reported runtime string `4.14.190-perf+`. The trailing `+` is a runtime build suffix and is not itself proof of a specific source commit.

## Device configuration mapping

The `motorola-sm6150-devs/device_motorola_hanoip` LineageOS 20 `BoardConfig.mk` points to:

- Kernel source path: `kernel/motorola/sm6150`
- Base defconfig: `vendor/sdmsteppe-perf_defconfig`
- Motorola common extension: `vendor/ext_config/moto-sdmsteppe.config`
- Hanoip-specific extension: `vendor/ext_config/moto-sdmmagpie-hanoip.config`
- Debugfs extension: `vendor/debugfs.config`
- LineageOS extension: `vendor/ext_config/lineage-moto-sdmsteppe.config`

The exact Motorola tag contains the first four listed config files. Its Hanoip extension sets `CONFIG_HANOIP_DTB=y` and includes device-specific driver and peripheral configuration choices.

The LineageOS extension is present in the community kernel fork's `lineage-20` branch, not in the Motorola release tag. It sets `CONFIG_LOCALVERSION="-lineageos"`, enables automatic local versioning and Clang LTO. Therefore, distinguish the stock Motorola source/configuration from the additional community build configuration.

A second community tree, `motorola-sm6150-devs/android_device_motorola_hanoip`, uses a different `BoardConfig.mk` convention and names `vendor/hanoip_defconfig`. That path was not found at the exact Motorola tag during this audit. Do not mix the two device-tree configurations without reconciling their intended kernel branch and source layout.

## Findings table

| Check | Result | Meaning |
|---|---|---|
| Exact XT2135-2 firmware source issue | Confirmed | Motorola issue #574 names the matching RETEU firmware package |
| Exact Motorola source tag | Confirmed | `MMI-S2RIS32.32-20-9-9-2-3` |
| Tag commit identifier | Partially recorded | Release page shows short ID `f6673a1`; full SHA not yet recorded |
| Kernel version | Confirmed | Source Makefile is 4.14.190 |
| Performance local version | Confirmed | Base defconfig sets `-perf` |
| Hanoip config extension | Confirmed | `CONFIG_HANOIP_DTB=y` in `moto-sdmmagpie-hanoip.config` |
| Device-tree config chain | Mostly matched | Motorola tag has base + Motorola + Hanoip + debugfs configs; LineageOS overlay comes from community fork |
| Exact binary/source reproducibility | Not tested | Requires build provenance, toolchain and output comparison |
| Android 17 / One UI 9 suitability | Not established | Kernel age and vendor integration remain major compatibility gates |

## Implications for OneUI-G60

1. The project can now cite an exact Motorola kernel tag instead of describing the target source revision as unknown.
2. The kernel is confirmed as a legacy 4.14.190 Android 12-era kernel. The source tag itself does not provide an Android 17 port.
3. The community device tree's Hanoip-specific config gives us a reliable historical hardware configuration reference.
4. Android 17 support would require a separate engineering assessment of kernel interfaces, vendor modules, HALs, graphics/media drivers, SELinux and VINTF. No compatibility claim should be inferred from source publication alone.
5. Do not upload proprietary firmware, vendor modules, extracted blobs, signing keys or other restricted artifacts to this public repository.

## Next research gate

The next step is to map the source-side device tree and vendor interface boundary: compare the Hanoip DTB selection and module list with the device-tree VINTF manifest, vendor HAL versions and firmware provenance. This remains documentation work; no device modification or flashing is required.

## References

- Motorola exact-build source request and maintainer response: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Exact Motorola source tag: https://github.com/MotorolaMobilityLLC/kernel-msm/releases/tag/MMI-S2RIS32.32-20-9-9-2-3
- Motorola kernel source repository: https://github.com/MotorolaMobilityLLC/kernel-msm
- Hanoip device configuration: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Community kernel configuration: https://github.com/motorola-sm6150-devs/kernel_motorola_sm6150/tree/lineage-20
