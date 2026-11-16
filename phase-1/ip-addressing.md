# Phase 1 — IP Addressing Plan

## Overview

This document records the IPv4 addressing plan for the simulated headquarters LAN in GNS3. Phase 1 is **Layer 2 only**: VLANs are configured, but there is no router or inter-VLAN routing yet.

- IPv4 ranges: RFC 1918 private addresses
- Subnet mask for each VLAN: `255.255.255.0` (`/24`)
- Gateway convention: `.1` in each subnet, **reserved but not active until Phase 2**
- Endpoint addressing in Phase 1: static, configured in VPCS

## VLAN and subnet allocation

| VLAN ID | Department / purpose | Subnet | Reserved gateway | Usable host range | Broadcast |
| --- | --- | --- | --- | --- | --- |
| 10 | IT | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.1–192.168.10.254` | `192.168.10.255` |
| 20 | HR | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.1–192.168.20.254` | `192.168.20.255` |
| 30 | Finance | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.1–192.168.30.254` | `192.168.30.255` |
| 40 | Engineering | `192.168.40.0/24` | `192.168.40.1` | `192.168.40.1–192.168.40.254` | `192.168.40.255` |
| 50 | Servers | `192.168.50.0/24` | `192.168.50.1` | `192.168.50.1–192.168.50.254` | `192.168.50.255` |
| 99 | Management (reserved) | `192.168.99.0/24` | `192.168.99.1` | `192.168.99.1–192.168.99.254` | `192.168.99.255` |

> **Phase 1 note:** These gateway IPs are planned addresses only. Entering a gateway in VPCS does not create a router. Cross-VLAN traffic is expected to fail until a router with these gateways is deployed.

## Endpoint assignments

| Host | VLAN | IPv4 address | Prefix | Configured gateway | Connected to |
| --- | --- | --- | --- | --- | --- |
| `IT-PC1` | 10 | `192.168.10.10` | `/24` | `192.168.10.1` (reserved) | `HQ-ACCESS-SW1` Ethernet1 |
| `IT-PC2` | 10 | `192.168.10.11` | `/24` | `192.168.10.1` (reserved) | `HQ-ACCESS-SW2` Ethernet1 |
| `HR-PC1` | 20 | `192.168.20.10` | `/24` | `192.168.20.1` (reserved) | `HQ-ACCESS-SW1` Ethernet2 |
| `FIN-PC1` | 30 | `192.168.30.10` | `/24` | `192.168.30.1` (reserved) | `HQ-ACCESS-SW2` Ethernet2 |
| `ENG-PC1` | 40 | `192.168.40.10` | `/24` | `192.168.40.1` (reserved) | `HQ-ACCESS-SW2` Ethernet3 |
| `SRV-01` | 50 | `192.168.50.10` | `/24` | `192.168.50.1` (reserved) | `HQ-CORE-SW1` Ethernet2 |

`SRV-01` is a **VPCS placeholder**, not an operational DNS, DHCP, or web server in Phase 1.

## VPCS configuration commands

Use the console for each named VPCS node. `save` stores the node's startup configuration.

```text
IT-PC1> ip 192.168.10.10/24 192.168.10.1
IT-PC1> save

IT-PC2> ip 192.168.10.11/24 192.168.10.1
IT-PC2> save

HR-PC1> ip 192.168.20.10/24 192.168.20.1
HR-PC1> save

FIN-PC1> ip 192.168.30.10/24 192.168.30.1
FIN-PC1> save

ENG-PC1> ip 192.168.40.10/24 192.168.40.1
ENG-PC1> save

SRV-01> ip 192.168.50.10/24 192.168.50.1
SRV-01> save
```

To verify an endpoint, execute `show ip` in its VPCS console.

## Future phases

- Phase 2: Enable inter-VLAN routing using an Ubuntu or VyOS router, with `.1` as the VLAN gateway addresses.
- Phase 3: Replace `SRV-01` with a real Linux VM; add DHCP, DNS, and internal services.
- Branch office: Use separate non-overlapping subnets (for example, `192.168.110.0/24`), to support routed WAN connectivity.
