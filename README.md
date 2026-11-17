# Enterprise Network Infrastructure Lab

## Overview

This project simulates an enterprise network using GNS3. The goal is to design, configure, secure, and manage a multi-site network while gaining hands-on experience with network engineering and systems administration.

The network consists of multiple departments separated by VLANs, with plans to implement centralized network services, firewalls, and connectivity between headquarters and a branch office.

The project currently covers VLAN segmentation and inter-VLAN routing using an Ubuntu Linux router.

## Technologies Used

- GNS3
- Ethernet Switching and VLANs
- TCP/IP Networking
- Wireshark
- Ubuntu Linux
- Netplan
- Linux IP Forwarding
- Python (planned)

## Network Architecture

The headquarters network consists of:

- 1 core Ethernet switch
- 2 access Ethernet switches
- 5 virtual workstations
- 1 virtual server placeholder
- 1 Ubuntu Linux router
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

### Inter-VLAN Routing

Phase 2 introduces an Ubuntu Linux router using a **router-on-a-stick** configuration.

The router connects to the core switch through an 802.1Q trunk and uses VLAN subinterfaces configured through Netplan.

Each VLAN has its own default gateway:

| VLAN | Default Gateway |
|---|---|
| 10 | 192.168.10.1 |
| 20 | 192.168.20.1 |
| 30 | 192.168.30.1 |
| 40 | 192.168.40.1 |
| 50 | 192.168.50.1 |
| 99 | 192.168.99.1 |

IPv4 forwarding enables communication between VLANs, while the Linux routing table maintains routes to each directly connected subnet.

## Project Phases

- **Phase 1 — LAN Configuration:** Configure switches, VLANs, IP addressing, and test network connectivity.
- **Phase 2 — Inter-VLAN Routing:** Implement routing between departments using an Ubuntu Linux router, Netplan VLAN interfaces, and IPv4 forwarding.
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
└── phase-2/
    ├── topology.png
    ├── router-configuration.md
    ├── routing-table.md
    └── connectivity-tests.md
```

## Testing

Network connectivity is tested using VPCS ping commands, Linux networking utilities, and Wireshark packet captures.

**Phase 1** focuses on verifying communication between devices within the same VLAN and confirming isolation between different VLANs.

**Phase 2** focuses on verifying inter-VLAN routing, including:

- Connectivity between departmental VLANs.
- Reachability of default gateways.
- IPv4 forwarding between VLAN interfaces.
- Routing table verification using `ip route`.
- Packet tracing between different subnets.
- Wireshark analysis of 802.1Q traffic.

Test procedures and results are documented in the `connectivity-tests.md` files for each phase.

## Project Goals

- Gain practical experience designing enterprise networks.
- Understand VLAN segmentation and network routing.
- Configure Linux-based routing infrastructure.
- Implement network security and monitoring.
- Develop Linux networking and automation skills.
- Document network configurations and troubleshooting procedures.
