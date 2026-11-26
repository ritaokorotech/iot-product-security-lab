# IoT Firmware Security Assessment

## 1. Assessment Overview

- **Date:** November 26, 2026
- **Target:** OWASP IoTGoat v1.0
- **Environment:** Isolated VirtualBox Lab
- **Assessment Type:** Static Firmware Analysis
- **Tools:** Kali Linux, Binwalk and Linux CLI

## 2. Assessment Objectives

- Analyze and extract IoT firmware.
- Identify embedded filesystems.
- Identify operating system and software versions.
- Perform software composition analysis.
- Identify potential security risks.
- Provide remediation recommendations.

## 3. Firmware Analysis Results

### Methodology

The analysis used a local copy of the IoTGoat x86 VMDK. Binwalk was used to identify embedded data and extract the SquashFS filesystem. Linux command-line inspection of the extracted filesystem identified operating system and component versions and produced a package inventory. A SHA-256 digest was recorded for the analyzed VMDK copy.

### Evidence and integrity

- [Firmware SHA-256 record](../firmware-analysis/firmware-hash.txt)
- [Software package inventory](../firmware-analysis/software-inventory.txt)
- [Supporting screenshots](../screenshots/)

The digest identifies the analyzed copy; it does not establish authenticity or equivalence to a trusted upstream release. Comparison against a trusted reference remains pending. Individual screenshot filenames and repository links have not been independently verified.

### Extraction results

Successfully analyzed and extracted the IoTGoat firmware image using Binwalk.

Identified components:

- VMware disk image (VMDK)
- EXT2 filesystem
- SquashFS filesystem
- XZ compressed data
- Linux kernel modules
- BusyBox utilities
- Lua application scripts

## 4. Software Composition Analysis

Successfully identified 122 software packages within the extracted firmware.

| Component | Identified Version |
|---|---|
| IoTGoat | v1.0 |
| OpenWrt | 18.06.2 |
| Linux Kernel | 4.14.95 |
| BusyBox | 1.28.4-2 |
| Dropbear SSH | 2017.75-7.1 |
| uHTTPd | 2018-11-28-cdfc902a-2 |
| Architecture | i386_pentium4 |

## 5. Initial Security Findings

| Observation | Evidence status | Potential risk |
|---|---|---|
| Legacy operating system and kernel versions | Versions identified; CVE applicability pending | Exposure to unpatched vulnerabilities |
| Legacy software dependencies | Package versions identified; CVE applicability pending | Vulnerable components may be present |
| Legacy SSH algorithms | Confirmed by runtime enumeration; F003 | Reduced cryptographic assurance |
| SSH password authentication enabled | Configuration observed; F002 | Unauthorized access if credentials are compromised |
| Root password authentication permitted | Configuration observed; F002 | Privileged account compromise |

The SSH observations are cross-references to the [initial security assessment](security-assessment.md), not additional independently confirmed firmware vulnerabilities. Package age alone does not establish exploitability. Vendor patches, build configuration, reachability and affected functionality must be evaluated before assigning CVE-based severity.

## 6. Security Recommendations

- Upgrade firmware to a supported version.
- Update outdated software dependencies.
- Disable deprecated cryptographic algorithms.
- Implement SSH key-based authentication.
- Restrict direct root access.
- Develop a Software Bill of Materials (SBOM).
- Implement continuous vulnerability monitoring.
- Verify firmware integrity using SHA-256 hashes.

### Proposed remediation acceptance criteria

| Control | Acceptance criteria (pending) |
|---|---|
| Component updates | Updated inventory records supported versions; applicable CVEs are reassessed and residual risks documented. |
| SSH hardening | F002 and F003 acceptance criteria in the initial assessment pass. |
| SBOM and monitoring | A structured component inventory records versions and available identifiers; vulnerability matches are reviewed for applicability. |
| Firmware integrity | A digest or signature is checked against a trusted reference where available; provenance and verification results are documented. |

## 7. Assessment Status

### Completed

- Firmware identification
- Firmware extraction
- Filesystem analysis
- Software composition analysis
- Software package inventory
- Initial security risk identification
- SHA-256 digest recorded for the analyzed VMDK copy

### Future Work

- CVE identification and validation
- Firmware authenticity and integrity verification against a trusted reference
- Hardcoded credential assessment
- Firmware update security assessment
- Remediation and retesting

## 8. Conclusion

The assessment successfully identified embedded filesystems, legacy software component versions and potential security risks within the IoTGoat firmware.

This project demonstrates practical skills in embedded product security, static firmware analysis, software composition analysis and vulnerability assessment.

All testing was conducted in an isolated, authorized laboratory environment.
