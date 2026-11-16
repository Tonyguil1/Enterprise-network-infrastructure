# Enterprise Network Infrastructure Lab

## Overview

This project simulates an enterprise network using GNS3. The goal is to design, configure, secure, and manage a multi-site network while gaining hands-on experience with network engineering and systems administration.

The network consists of multiple departments separated by VLANs, with plans to implement inter-VLAN routing, centralized network services, firewalls, and connectivity between headquarters and a branch office.

## Technologies Used

- GNS3
- Ethernet Switching and VLANs
- TCP/IP Networking
- Wireshark
- Ubuntu Linux (planned)
- Python (planned)

## Network Architecture

The headquarters network consists of:

- 1 core Ethernet switch
- 2 access Ethernet switches
- 5 virtual workstations
- 1 virtual server placeholder
- 6 VLANs for departmental segmentation

### VLAN Configuration

| VLAN | Department | Subnet |
|---|---|---|
| 10 | IT | 192.168.10.0/24 |
| 20 | HR | 192.168.20.0/24 |
| 30 | Finance | 192.168.30.0/24 |
| 40 | Engineering | 192.168.40.0/24 |
| 50 | Servers | 192.168.50.0/24 |
| 99 | Management | 192.168.99.0/24 |

## Project Phases

- **Phase 1 — LAN Configuration:** Configure switches, VLANs, IP addressing, and test network connectivity.
- **Phase 2 — Inter-VLAN Routing:** Implement routing between departments using a Linux router.
- **Phase 3 — Network Services:** Deploy DHCP, DNS, and internal servers.
- **Phase 4 — Network Security:** Implement firewall rules, access controls, and monitoring.
- **Phase 5 — Multi-Site Networking:** Connect headquarters and a branch office using dynamic routing and a site-to-site VPN.
- **Phase 6 — Automation:** Develop Python scripts for network testing and management.

## Repository Structure

```text
enterprise-network-lab/
├── README.md
├── phase-1/
│   ├── topology.png
│   ├── ip-addressing.md
│   ├── vlan-configuration.md
│   └── connectivity-tests.md
```

## Testing

Network connectivity is tested using VPCS ping commands and Wireshark packet captures.

Phase 1 focuses on verifying communication between devices within the same VLAN and confirming isolation between different VLANs.

Test results are documented in `phase-1/connectivity-tests.md`.

## Project Goals

- Gain practical experience designing enterprise networks.
- Understand VLAN segmentation and network routing.
- Implement network security and monitoring.
- Develop Linux networking and automation skills.
- Document network configurations and troubleshooting procedures.
