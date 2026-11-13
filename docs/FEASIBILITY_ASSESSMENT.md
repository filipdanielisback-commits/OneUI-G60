# Android 17 / One UI 9 feasibility assessment

**Assessment date:** 2026-11-13  
**Target:** Motorola Moto G60 XT2135-2, 6 GB RAM  
**Reported baseline:** Android 12, build `S2RIS32.32-20-9-9-2-3`, kernel `4.14.190-perf+`  
**Assessment status:** Preliminary research only — no build or device test performed

## Executive assessment

A direct transplant of a complete Samsung One UI 9 system onto the current Moto G60 software base is **high risk and not established as feasible**.

Samsung identifies One UI 9 with Android 17. The target phone is confirmed by the user to run Android 12. This is a five-generation platform gap involving framework APIs, system services, security policy, vendor interfaces and hardware support.

The phone's reported 4.14.190 kernel is also much older than the Android common GKI kernel generations listed for Android 17. However, the Android GKI compatibility matrix describes GKI kernels; it is not by itself a proof that every legacy non-GKI device kernel cannot run a newer Android userspace. The exact kernel/vendor integration must be investigated separately.

**Current conclusion:** feasibility is unknown and technically challenging. Do not describe this as a working port, and do not create or flash an image at this stage.

## Evidence reviewed

| Item | Finding | Confidence / limitation |
|---|---|---|
| One UI 9 platform | Samsung's official announcements identify Android 17 as its base | Official vendor documentation |
| Target OS | Android 12, confirmed by user from Settings | User-confirmed |
| Target kernel | `4.14.190-perf+` | User-reported version string; exact source revision unknown |
| Community device tree | hanoip device trees exist; one visible branch is `lineage-20` | Community source reference, not an Android 17 port |
| Community target status | Moto-Common target index marks hanoip as Dropped | Repository-maintainer status; historical context |
| Kernel source | Motorola published-source issue exists for the reported firmware family; exact matching source revision/config still needs confirmation | Source mapping incomplete |
| Treble / VNDK | Not yet checked on the exact XT2135-2 installation | Critical unknown |

## Kernel compatibility: important nuance

AOSP's Android common-kernel documentation lists supported GKI kernel generations for Android 17, including `android17-6.18` and selected earlier GKI branches. The Moto G60's reported 4.14 kernel is outside that listed GKI matrix.

This comparison is a warning, not a direct compatibility verdict:

- The Moto G60's reported kernel is a legacy device/vendor kernel, not established here as a GKI kernel.
- GKI guarantees and KMI compatibility rules apply to the documented GKI branches.
- A newer Android userspace also depends on vendor HALs, VINTF declarations, SELinux policy, binder interfaces, graphics, media, camera, audio, radio and device-specific drivers.
- Replacing a device kernel or vendor stack is a major independent engineering project and cannot be assumed possible from a version-number comparison.

Reference: https://source.android.com/docs/core/architecture/kernel/android-common

## Most important next compatibility gate: Treble and VNDK

Android's official GSI guidance says to check:

1. Whether the device reports `ro.treble.enabled=true`.
2. Whether its vendor linker namespace configuration indicates cross-version VNDK support (including `namespace.default.isolated` in the relevant `ld.config.*.txt` file).
3. Whether the system and vendor architectures match.

The exact checks must be performed on the XT2135-2. No values have been collected yet, so this repository does not claim that the phone is cross-version GSI-compatible.

Reference: https://developer.android.com/topic/generic-system-image

A GSI is an AOSP system-image compatibility test, not a Samsung One UI port. Passing GSI checks would be useful evidence about the Android platform/vendor boundary, but it would not prove that Samsung framework components work.

## Existing hanoip sources

Community device trees for Moto G40 Fusion / Moto G60 provide useful historical references:

- https://github.com/motorola-sm6150-devs/android_device_motorola_hanoip
- https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- https://github.com/TeamWin/android_device_motorola_hanoip
- https://github.com/moto-common/android_device_motorola_targets

The visible device-tree references include a `lineage-20` branch, and the Moto-Common target index lists hanoip as Dropped. These sources can help us understand the device configuration and prior Android 13-era work, but they are not an Android 17 device bring-up and should not be copied wholesale or treated as maintained build instructions.

## Compatibility gates

| Gate | Required evidence | Status |
|---|---|---|
| Exact target identity | XT2135-2, regional SKU and firmware | Partially documented |
| Android platform boundary | Treble and vendor namespace properties | Not checked |
| Vendor interfaces | VINTF manifest/matrix and HAL versions | Not checked |
| Kernel provenance | Exact Motorola source commit, config and patch set | Not matched |
| Graphics | GPU kernel driver, userspace EGL/Vulkan stack and Android 17 support | Not checked |
| Audio / camera / radio | HAL and vendor dependency mapping | Not checked |
| Samsung framework | Exact One UI 9 reference build and Samsung service dependencies | Not mapped |
| Licensing | Component-by-component redistribution review | Not completed |

## Research strategy

1. **Read-only device inventory:** when a PC is available, collect non-sensitive Android properties and VINTF information from the exact phone. Do not change partitions or boot state.
2. **Source provenance:** match the exact Motorola kernel source revision and review the hanoip tree history.
3. **AOSP platform feasibility:** assess whether the existing vendor stack can support a newer Android platform, using official Treble/VNDK requirements.
4. **Samsung dependency map:** choose an official One UI 9 reference device/build and identify framework-only versus hardware-bound components.
5. **Prototype decision:** only after the earlier gates have evidence, decide whether to pursue a limited feature prototype, an AOSP base with selected original implementations, or stop due to incompatibility.

## Safety and project integrity

- This is research only. No flashable image exists.
- Do not upload Samsung/Motorola proprietary firmware, restricted binaries, signing keys or extracted vendor blobs to this public repository without redistribution permission.
- Do not attempt device modifications based on this preliminary assessment.
- Any future device testing needs a verified backup and recovery plan, exact model matching and documented review.
- Mark a feature Working only after reproducible tests on XT2135-2.

## References

- Android common kernels / GKI compatibility: https://source.android.com/docs/core/architecture/kernel/android-common
- Android Generic System Image requirements: https://developer.android.com/topic/generic-system-image
- Samsung One UI 9 announcement: https://news.samsung.com/us/samsung-launches-one-ui-9-beta-galaxy-s26-series-users/
- Samsung One UI 9 rollout announcement: https://news.samsung.com/us/samsung-begins-official-rollout-one-ui-9-latest-galaxy-experiences-devices/
- Moto-Common hanoip target status: https://github.com/moto-common/android_device_motorola_targets
