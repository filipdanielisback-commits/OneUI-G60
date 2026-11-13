# One UI 9 research

## Goal

Study One UI 9 as a software platform and evaluate whether selected, legally usable components or original compatibility implementations could run on the Motorola Moto G60 (XT2135-2).

One UI 9 is based on Android 17, while the target Moto G60 currently runs Android 12. This creates a major platform-version gap that must be investigated before any porting work. This is a research objective, not a claim that a port exists. No bootable image or working port is available.

## Establish the reference platform

Samsung's official materials now confirm that One UI 9 is based on **Android 17**. Samsung announced the One UI 9 beta for the Galaxy S26 series on May 12, 2026, describing it as built on Android 17. Samsung announced the broader official rollout on September 16, 2026.

Sources:
- Samsung Newsroom, One UI 9 beta announcement (May 12, 2026): https://news.samsung.com/us/samsung-launches-one-ui-9-beta-galaxy-s26-series-users/
- Samsung Newsroom, official rollout announcement (September 16, 2026): https://news.samsung.com/us/samsung-begins-official-rollout-one-ui-9-latest-galaxy-experiences-devices/
- Samsung UK, One UI overview and Android 17 base: https://www.samsung.com/uk/mobile-phone-buying-guide/what-is-one-ui/

Before drawing device-specific technical conclusions, record the following for the exact reference build being studied:

- Official One UI 9 announcement or product documentation.
- Android platform version used by that specific One UI 9 build.
- Reference Samsung device model and region.
- Exact firmware build identifier and release date.
- Required Samsung framework services and libraries.
- License and redistribution terms for every component considered.

Do not infer the Android base version from a rumor, a package filename or a different One UI release. One UI builds can vary by device and release channel.

## Compatibility questions

- Which Android framework APIs and services does the target release require?
- Which Samsung framework extensions are required by individual applications?
- Which components depend on Samsung-specific hardware, security services or vendor interfaces?
- Can an equivalent feature be implemented using AOSP interfaces and original code?
- Which dependencies are device-specific and cannot reasonably be reused on hanoip?

## Evidence standard

Every finding should include a source URL, date checked, affected version, confidence level and a short explanation. Distinguish official documentation, source-code evidence, community reports and hypotheses.

## Current status

| Research item | Status |
|---|---|
| Official One UI 9 release documentation | Confirmed from Samsung announcements |
| Android base version | Android 17, confirmed by Samsung for One UI 9 |
| Reference Samsung device/build | Not selected |
| Framework dependency map | Not started |
| Component licensing review | Not started |
| Moto G60 compatibility assessment | Not started |
