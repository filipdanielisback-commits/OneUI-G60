# Target device research: Motorola Moto G60 XT2135-2

## Confirmed from Motorola documentation

Motorola's EU declaration identifies model **XT2135-2** as **moto g60** (also called Moto G60 Vision in some Motorola support listings). The same support listing distinguishes XT2135-1 as a separate Moto G60 model entry.

- Motorola EU declaration: https://en-us.support.motorola.com/ci/fattach/get/79315385/1664922506/redirect/1/filename/XT2135-2_Moto%2BG60%2BVision%2BMC38C%2BRED%2BDoC.pdf
- Motorola model listing: https://en-us.support.motorola.com/app/answers/detail/a_id/167829/~/declarations-of-conformity

## Platform research leads

Community device trees identify the shared Moto G40 Fusion / Moto G60 platform codename as `hanoip`. Treat this as a research lead, not proof that every G40 Fusion configuration is compatible with XT2135-2.

- Device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- TeamWin device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Community target status: https://github.com/moto-common/android_device_motorola_targets

A community device specification lists Qualcomm Snapdragon 732G (SM6150 family), Adreno 618 and 4/6 GB memory for the shared device family. Verify all properties against the exact phone before relying on them.

## Firmware and software baseline

- Firmware/build reported by user: `S2RIS32.32-20-9-9-2-3`
- Android security update reported by user: **November 1, 2023**
- Motorola kernel-source request references a firmware package named `XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3...`. This links the build identifier with XT2135-2 / hanoip and an Android 12 firmware package; it does not independently verify the software currently installed on the phone.

- Motorola kernel source request: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574

## Before implementation

Record the following from the actual device, with personal identifiers removed:

- Android version and security patch level shown in Settings > About phone.
- Full model number and build number shown in Settings.
- SoC / board identifiers from trustworthy system information.
- Kernel version.
- Available storage and RAM variant.
- Whether the installed firmware exactly matches the reported build.

Do not publish IMEI, serial number, phone number, account identifiers or unredacted diagnostic logs.

## Verification status

| Property | Current status |
|---|---|
| Model XT2135-2 | Confirmed by Motorola regulatory document |
| Shared codename hanoip | Supported by community device-tree references |
| Snapdragon 732G / Adreno 618 | Community specification; verify on device |
| 6 GB RAM | User-reported target variant |
| Firmware S2RIS32.32-20-9-9-2-3 | User-reported; matching Motorola kernel-source request exists |
| Android security update November 1, 2023 | User-reported |
| Exact installed Android version | Android 12 indicated by matching firmware package; verify on device |
| Partition layout / boot configuration | Not yet verified |


## Milestone 2: device-tree and kernel-source audit

### Device tree findings

The public `motorola-sm6150-devs/device_motorola_hanoip` repository identifies itself as an Android device tree for Moto G40 Fusion / Moto G60 (`hanoip`). The TeamWin `android_device_motorola_hanoip` repository also documents G40/G60 support. These are useful upstream references, but neither alone proves that its configuration is correct for every XT2135-2 regional SKU or the exact RETEU firmware.

- https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- https://github.com/TeamWin/android_device_motorola_hanoip
- https://github.com/moto-common/android_device_motorola_targets

The Moto-Common targets index currently lists `hanoip` as **Dropped**. Treat old device trees as historical reference material, not as a maintained supported target.

### Kernel-source findings

Motorola's official `MotorolaMobilityLLC/kernel-msm` repository is the relevant public kernel source repository for MSM-based Motorola devices:

- https://github.com/MotorolaMobilityLLC/kernel-msm

The exact firmware source request for `XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3...` is closed and labeled `publishedoss code is published`:

- https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574

A branch search in the official repository found these related Android 12 branches:
- `android-12-release-s2ris32.32-20-7-7`
- `android-12-release-s2ris32.32-20-7-11`
- `android-12-release-s2ris32.32-20-7-15`
- `android-12-release-s2ris32.32-20-9-9-2-1`

It did **not** return a branch named for the exact `...-2-3` build. This does not prove that no matching source tag or commit exists; it means the exact branch has not yet been identified. The kernel repository's Releases page reports no packaged releases, so do not treat that page as proof of source availability.

### Current engineering decision

- Use the hanoip device trees as references for understanding product configuration, HAL declarations, init files, partition metadata and proprietary-file manifests.
- Do not copy a tree wholesale into this project or assume G40 Fusion and XT2135-2 are interchangeable without checking SKU-specific properties.
- Do not select a kernel branch based only on a similar build number. First establish the exact source revision, kernel version, toolchain and matching device configuration.
- Do not mark Wi-Fi, battery, audio, GPU, CPU or apps as working based on source inspection alone.

### Audit status

| Area | Result |
|---|---|
| Shared device tree reference | Found |
| TWRP device configuration reference | Found |
| Current community target status | Dropped |
| Exact firmware source request | Closed; labeled code published |
| Exact `...-2-3` kernel branch | Not identified in branch search |
| Matching kernel commit / defconfig | Not yet verified |
| XT2135-2 SKU-specific configuration | Not yet verified |
| Build readiness | Not established |
