# IoT Product Security Lab

A hands-on product security lab using Kali Linux, OWASP IoTGoat, and Oracle VirtualBox. This project documents how to build an isolated environment for IoT security testing, network reconnaissance, firmware analysis, and vulnerability assessment.
## Executive Summary

This independent product security research project demonstrates an initial security assessment of an intentionally vulnerable embedded IoT device using OWASP IoTGoat.

### Key Accomplishments

- Embedded device attack surface assessment
- Network reconnaissance and penetration testing
- Threat modeling using STRIDE
- Firmware extraction and static analysis using Binwalk
- Software component inventory of 122 packages
- SSH cryptographic security assessment
- Web authentication security testing
- Vulnerability identification and remediation planning

### Key Findings

- Weak administrator password policy
- Root password SSH configuration and lab reachability confirmed
- Deprecated SSH cryptographic algorithms
- Legacy firmware and software components requiring further vulnerability assessment

All testing was conducted within an isolated, controlled laboratory environment.

## Initial security assessment (November 25, 2026)

Testing was limited to the isolated IoTGoat lab. Tools: Nmap 7.99 and Burp Suite Community Edition 2026.8.

| Finding | Status | Provisional severity |
| --- | --- | --- |
| F001: Weak administrator password policy | Confirmed by user test | Medium |
| F002: Root password SSH configuration | Configuration and lab reachability confirmed | Medium |
| F003: Deprecated SSH algorithms | Confirmed by enumeration | Medium |

See [security assessment report](./reports/security-assessment.md) for evidence, limitations and recommendations.

## Project Documentation

- [Security Assessment Reports](./reports/)
- [Firmware Analysis](./firmware-analysis/)
- [Threat Modeling](./threat-modeling/)
- [Security Testing Evidence](./screenshots/)

- [Lab Setup Guide](./lab-setup.md)

## Project Status

- [x] Isolated IoT security laboratory
- [x] Network reconnaissance and service discovery
- [x] Web authentication security testing
- [x] SSH security assessment
- [x] Firmware extraction and initial static analysis
- [x] Software component inventory
- [x] Threat modeling and STRIDE assessment
- [x] Initial security assessment reports
- [ ] CVE identification and validation
- [ ] Hardcoded credentials and secrets review
- [ ] Remediation verification and retesting

## Lab architecture

```text
        VirtualBox Internal Network: iot-lab
        Subnet: 192.168.100.0/24

  Kali Linux <--------------------> OWASP IoTGoat
  192.168.100.20                    192.168.100.10
  Security testing machine          Vulnerable target
```

| Component | IP address | Interface |
| --- | --- | --- |
| Kali Linux | 192.168.100.20/24 | eth0 |
| IoTGoat | 192.168.100.10/24 | br-lan |

No internet connection or default gateway is required for this lab.

## Tools and prerequisites

- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Kali Linux prebuilt VirtualBox image](https://www.kali.org/get-kali/)
- [OWASP IoTGoat x86 VMDK image](https://github.com/OWASP/IoTGoat/releases)
- A host capable of running x86 virtual machines. These instructions assume an x86-compatible VirtualBox environment.

IoTGoat is intentionally vulnerable firmware based on OpenWrt, designed for security education.

Additional tools: Nmap, Burp Suite Community Edition, Binwalk and OWASP Threat Dragon.

## Firmware Analysis and Next Steps

### Completed
- Firmware analysis using Binwalk
- SquashFS filesystem extraction
- SHA-256 hashing of the analyzed VMDK copy
- Identification of OpenWrt 18.06.2
- Software inventory of 122 packages
- Firmware security assessment
- Threat modeling and STRIDE assessment
- Security testing documentation

### Next Session
- CVE identification and validation
- Hardcoded credentials and secret review
- Firmware configuration security review
- Vulnerability risk scoring using CVSS
- Remediation implementation and retesting

## Security precautions

- Keep IoTGoat on the isolated internal network.
- Do not expose IoTGoat to the public internet or use bridged networking.
- Disable unnecessary shared folders, USB access, shared clipboard, and drag-and-drop.
- Only test systems you own or have explicit permission to assess.

## References

- [OWASP IoTGoat](https://github.com/OWASP/IoTGoat)
- [Kali Linux](https://www.kali.org/)
- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Nmap documentation](https://nmap.org/docs.html)


