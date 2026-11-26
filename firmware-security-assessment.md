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

| Finding | Potential Risk |
|---|---|
| Outdated operating system | Unpatched vulnerabilities |
| Outdated software dependencies | Known vulnerability exposure |
| Legacy SSH algorithms | Weak cryptographic protection |
| SSH password authentication enabled | Unauthorized access |
| Root password authentication permitted | Privileged account compromise |

## 6. Security Recommendations

- Upgrade firmware to a supported version.
- Update outdated software dependencies.
- Disable deprecated cryptographic algorithms.
- Implement SSH key-based authentication.
- Restrict direct root access.
- Develop a Software Bill of Materials (SBOM).
- Implement continuous vulnerability monitoring.
- Verify firmware integrity using SHA-256 hashes.

## 7. Assessment Status

### Completed

- Firmware identification
- Firmware extraction
- Filesystem analysis
- Software composition analysis
- Software package inventory
- Initial security risk identification

### Future Work

- CVE identification and validation
- Firmware integrity verification
- Hardcoded credential assessment
- Firmware update security assessment
- Remediation and retesting

## 8. Conclusion

The assessment successfully identified embedded filesystems, outdated software components and potential security risks within the IoTGoat firmware.

This project demonstrates practical skills in embedded product security, static firmware analysis, software composition analysis and vulnerability assessment.

All testing was conducted in an isolated, authorized laboratory environment.
