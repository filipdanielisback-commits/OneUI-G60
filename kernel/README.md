# Kernel research

## Exact Motorola source reference

The exact Motorola source tag for the reported Moto G60 XT2135-2 firmware `S2RIS32.32-20-9-9-2-3` has been identified:

- Repository: https://github.com/MotorolaMobilityLLC/kernel-msm
- Tag: `MMI-S2RIS32.32-20-9-9-2-3`
- Tag page: https://github.com/MotorolaMobilityLLC/kernel-msm/releases/tag/MMI-S2RIS32.32-20-9-9-2-3
- Motorola issue #574: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Tag page short commit identifier: `f6673a1`

The tag's source Makefile identifies Linux `4.14.190`, and the base performance defconfig sets `CONFIG_LOCALVERSION="-perf"`. This agrees with the user's reported runtime kernel family `4.14.190-perf+`.

## Hanoip configuration references

The matching LineageOS 20 device-tree branch points to `kernel/motorola/sm6150` and uses:

- `vendor/sdmsteppe-perf_defconfig`
- `vendor/ext_config/moto-sdmsteppe.config`
- `vendor/ext_config/moto-sdmmagpie-hanoip.config`
- `vendor/debugfs.config`
- `vendor/ext_config/lineage-moto-sdmsteppe.config`

The exact Motorola tag contains the base, Motorola, Hanoip and debugfs config files. The LineageOS-specific extension is supplied by the community kernel fork, not the Motorola tag. A second community device tree uses a different `vendor/hanoip_defconfig` naming convention; do not combine these configurations without checking their intended source branches.

See [the detailed kernel-source audit](../docs/KERNEL_SOURCE_AUDIT.md).

## Limits

- Identifying the source tag does not prove bit-for-bit reproducibility of the installed kernel.
- No kernel build or boot test has been performed.
- No compatibility with Android 17 or One UI 9 has been established.
- Do not add proprietary firmware, restricted modules, extracted vendor blobs or signing keys without permission to redistribute.

The next research task is to compare the Hanoip device configuration, DTB selection, module inventory, vendor HALs and VINTF declarations.