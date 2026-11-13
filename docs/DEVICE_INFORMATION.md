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
