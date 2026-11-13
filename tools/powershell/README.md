# PowerShell tools

This directory is for Windows-side helper scripts for repository maintenance and research bookkeeping.

## Script standards

- Explain purpose, inputs, outputs and prerequisites in a comment header.
- Default to read-only behavior.
- Never delete, overwrite or modify device partitions.
- Never store passwords, tokens, signing keys or personal device identifiers.
- Use explicit output paths and avoid silently changing the user's environment.
- Include a dry-run option for any future operation that changes files.
- Test scripts on sample files before using them with research data.

Planned helpers: repository preflight, metadata validation and checksum reporting. No device-flashing automation is planned here.
