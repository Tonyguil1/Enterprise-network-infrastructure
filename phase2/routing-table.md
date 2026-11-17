# Phase 2 — Routing Table Documentation

## Overview

This file documents the expected IPv4 routing table on `HQ-ROUTER-01` after router-on-a-stick is configured. Phase 2 uses **directly connected networks**, so no OSPF, static inter-VLAN routes, or Internet default route is required.


## Router and interface inventory

| Device | Parent NIC | Role |
| --- | --- | --- |
| `HQ-ROUTER-01` | `ens3` | 802.1Q trunk to core switch |
| `HQ-CORE-SW1` | Ethernet3 | Dot1Q/native VLAN 1 toward Ubuntu |

## VLAN and directly connected route allocation

| VLAN ID | Department / purpose | Destination subnet | Router interface | Router gateway |
| --- | --- | --- | --- | --- |
| 10 | IT | `192.168.10.0/24` | `ens3.10` | `192.168.10.1` |
| 20 | HR | `192.168.20.0/24` | `ens3.20` | `192.168.20.1` |
| 30 | Finance | `192.168.30.0/24` | `ens3.30` | `192.168.30.1` |
| 40 | Engineering | `192.168.40.0/24` | `ens3.40` | `192.168.40.1` |
| 50 | Servers | `192.168.50.0/24` | `ens3.50` | `192.168.50.1` |
| 99 | Management | `192.168.99.0/24` | `ens3.99` | `192.168.99.1` |

> Each route is added automatically when the VLAN interface receives its `/24` address. There is no next-hop router for a directly connected subnet.






