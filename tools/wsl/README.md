# WSL2 research environment

Use WSL2 with a supported Ubuntu distribution as the Linux-side environment for source inspection and Android build-system research.

## Environment record

Record the Windows version, WSL version, Ubuntu release, available memory and disk space, Java version and required build tools when the environment is established.

## Working practices

- Keep source checkouts and Linux build work inside the WSL2 Linux filesystem for better Linux tool compatibility.
- Keep a separate backup of important project notes and source changes.
- Install only tools required by a documented task.
- Prefer pinned tool versions and record installation commands.
- Do not run privileged or device-modifying commands unless a later, reviewed task specifically requires them.
- Do not store credentials or proprietary firmware in the public repository.

This document is a plan; it does not claim that a build environment has been installed or tested.
