# IoTGoat Firmware Analysis

## Overview

This assessment demonstrates static firmware analysis, filesystem extraction and software component identification using Kali Linux and Binwalk.

## Analysis Results

- Firmware: OWASP IoTGoat v1.0
- Operating System: OpenWrt 18.06.2
- Linux Kernel: 4.14.95
- Architecture: i386_pentium4
- Filesystem: SquashFS
- Identified Software Packages: 122

## Analysis Artifacts

- [Firmware SHA-256 Hash](./firmware-hash.txt)
- [Software Component Inventory](./software-inventory.txt)
- [Firmware Security Assessment](../reports/firmware-security-assessment.md)

## Security Considerations

The firmware hash records the integrity fingerprint of the analyzed VMDK copy. It does not independently establish firmware authenticity.

The software inventory supports vulnerability identification, dependency assessment and future SBOM development.

## Next Steps

- CVE identification and validation
- Hardcoded credentials and secrets review
- Firmware configuration assessment
- Software dependency risk assessment
- Remediation and retesting

All analysis was performed within a controlled laboratory environment.
