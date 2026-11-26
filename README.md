# IoT Product Security Lab

A hands-on product security lab using Kali Linux, OWASP IoTGoat, and Oracle VirtualBox. This project documents how to build an isolated environment for IoT security testing, network reconnaissance, firmware analysis, and vulnerability assessment.

## Project status

- [x] Configure Kali Linux
- [x] Configure OWASP IoTGoat
- [x] Establish an isolated internal network
- [x] Verify connectivity using ping
- [x] Network reconnaissance and service discovery
- [ ] Firmware analysis
- [x] Initial network and web vulnerability assessment
- [x] Initial remediation recommendations and security report
- [ ] Remediation verification and final assessment

## Tools and prerequisites

- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Kali Linux prebuilt VirtualBox image](https://www.kali.org/get-kali/)
- [OWASP IoTGoat x86 VMDK image](https://github.com/OWASP/IoTGoat/releases)
- A host capable of running x86 virtual machines. These instructions assume an x86-compatible VirtualBox environment.

IoTGoat is intentionally vulnerable firmware based on OpenWrt, designed for security education.

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

## 1. Set up Kali Linux

1. Download the prebuilt Kali Linux VirtualBox image.
2. Extract the downloaded archive.
3. Open VirtualBox and select **Open**.
4. Select the extracted Kali `.vbox` file.
5. With the VM powered off, open **Settings**.
6. Apply the recommended settings below.

| Setting | Recommended value |
| --- | --- |
| RAM | 4096 MB |
| Processors | 2 CPUs |
| Processing cap | 100% |
| Graphics controller | VMSVGA |
| Adapter 1 | Enabled |
| Attached to | Internal Network |
| Network name | `iot-lab` |
| Adapter type | Intel PRO/1000 MT Desktop (82540EM) |
| Promiscuous mode | Deny |
| Virtual cable connected | Enabled |
| Other network adapters | Disabled |

Start Kali. The default credentials for the standard prebuilt image are `kali` / `kali`, unless changed.

## 2. Set up OWASP IoTGoat

1. Download the `IoTGoat-x86.vmdk` image from the OWASP IoTGoat releases page. Extract it if necessary.
2. Open VirtualBox and click **New**.
3. Name the virtual machine `IoTGoat`.
4. Select Linux as the operating system. Oracle Linux (64-bit) was used in this lab.
5. Select **Use an existing virtual hard disk** and choose `IoTGoat-x86.vmdk`.
6. Finish creating the VM and open **Settings**.
7. Confirm that the VMDK is attached under the SATA controller. If an empty VDI was created, detach it and attach the downloaded VMDK instead.

| Setting | Value |
| --- | --- |
| RAM | 512 MB |
| Processors | 1 CPU (recommended) |
| Processing cap | 100% |
| Boot order | Hard Disk |
| Chipset | PIIX3 |
| TPM | None |
| I/O APIC | Enabled |
| Hardware clock in UTC | Enabled |
| UEFI | Disabled |
| Secure Boot | Disabled |
| Video memory | 16 MB |
| Graphics controller | VMSVGA |
| Storage controller | SATA |
| Virtual hard disk | `IoTGoat-x86.vmdk` |
| Audio | Disabled |
| USB | Disabled |
| Serial ports | Disabled |
| Shared folders | None |

### Network settings

| Setting | Value |
| --- | --- |
| Adapter 1 | Enabled |
| Attached to | Internal Network |
| Network name | `iot-lab` |
| Adapter type | Intel PRO/1000 MT Desktop (82540EM) |
| Promiscuous mode | Deny |
| Virtual cable connected | Enabled |
| Other network adapters | Disabled |

Both VMs must use exactly the same internal network name. Start IoTGoat and wait for the BusyBox shell.

## 3. Configure the IoTGoat IP address

Inspect the network interfaces:

```bash
ifconfig
```

Assign a static IPv4 address to the bridge interface:

```bash
ifconfig br-lan 192.168.100.10 netmask 255.255.255.0 up
```

Verify the address:

```bash
ifconfig br-lan
```

Expected IPv4 address: `192.168.100.10`.

**Note:** This configuration is temporary and may need to be repeated after rebooting IoTGoat.

## 4. Configure the Kali Linux IP address

Open a terminal and identify the network interface:

```bash
ip addr show
```

Create a persistent static connection using NetworkManager:

```bash
sudo nmcli con add type ethernet ifname eth0 con-name iot-lab ipv4.method manual ipv4.addresses 192.168.100.20/24 ipv4.never-default yes
```

Activate the connection:

```bash
sudo nmcli con up iot-lab
```

Verify the IP address:

```bash
ip addr show eth0
```

Expected IPv4 address: `192.168.100.20/24`.

The interface name `eth0` ends with the number zero, not the letter O. If the interface name differs on your system, substitute the correct name. If the connection already exists, activate it rather than creating a duplicate.

## 5. Verify connectivity

Keep both VMs running. From the Kali terminal, execute:

```bash
ping -c 4 192.168.100.10
```

Expected result: four packets transmitted, four received, and 0% packet loss.

**Lab result:** Successful ping connectivity was confirmed between Kali Linux and IoTGoat.

## 6. Troubleshooting

| Issue | Solution |
| --- | --- |
| Network is unreachable | Verify Kali has an IPv4 address and a route to the lab subnet. Use `ip addr show eth0` and `ip route`. |
| Kali loses its manually assigned IP | Configure the persistent NetworkManager profile described above. |
| Cannot find device `etho` | Use `eth0` with a numeric zero, or verify the actual interface name. |
| No bootable medium | Attach the downloaded IoTGoat VMDK rather than an empty VDI. |
| Ping fails | Verify both VMs are running, their IP addresses are correct, their internal network names match, and virtual cables are connected. |
| Mouse captured by VirtualBox | Press the configured Host key. In this Mac setup, it is the left Command key. |
| Kali displays a disconnected notification | Verify the static connection and ping the target. Internet access is not required. |

## 7. First security exercise: Network reconnaissance

**Status: Initial service discovery completed.**

From Kali, scan the authorized IoTGoat target:

```bash
nmap 192.168.100.10
```

Objectives:

- Identify open TCP ports and likely services.
- Understand the device's exposed network attack surface.
- Identify services that require further assessment.
- Document evidence, potential risks, and remediation recommendations.

SSH on TCP 22 and HTTPS on TCP 443 were confirmed. Open ports alone do not establish that a vulnerability exists.

## 8. Planned repository structure

```text
IoT-Product-Security-Lab/
├── README.md
├── screenshots/
├── reconnaissance/
├── firmware-analysis/
├── vulnerability-assessment/
├── remediation/
└── reports/
```

Add screenshots, scan outputs, and assessment reports as each exercise is completed. Remove personal information from screenshots before publishing.

## 9. Security precautions

- Keep IoTGoat on the isolated internal network.
- Do not expose IoTGoat to the public internet or use bridged networking.
- Disable unnecessary shared folders, USB access, shared clipboard, and drag-and-drop.
- Only test systems you own or have explicit permission to assess.

## 10. Skills demonstrated and learning goals

**Completed:** Virtualization, Linux administration, static IP configuration, network isolation, connectivity troubleshooting, service discovery, web authentication testing, SSH configuration review, cryptographic enumeration, and initial findings documentation.

**Planned:** Firmware analysis, further vulnerability assessment, and remediation verification.

## References

- [OWASP IoTGoat](https://github.com/OWASP/IoTGoat)
- [Kali Linux](https://www.kali.org/)
- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Nmap documentation](https://nmap.org/docs.html)


## 11. Initial security assessment (November 25, 2026)

Testing was limited to the isolated IoTGoat lab. Tools: Nmap 7.99 and Burp Suite Community Edition 2026.8.

| Finding | Status | Provisional severity |
| --- | --- | --- |
| F001: Weak administrator password policy | Confirmed by user test | Medium |
| F002: Root password SSH configuration | Configuration and lab reachability confirmed | Medium |
| F003: Deprecated SSH algorithms | Confirmed by enumeration | Medium |

See [[initial security assessment](reports/initial-assessment.md]) for evidence, limitations and recommendations.

## 12. Next session

Begin firmware analysis by checking whether Binwalk is installed. Firmware extraction, component inventory, secret review and remediation retesting remain pending.
