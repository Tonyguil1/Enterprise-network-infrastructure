# Phase 2 — Inter-VLAN Connectivity Tests

## Objective

Verify that the Ubuntu router `HQ-ROUTER-01` can reach endpoints on all configured VLANs and can forward traffic between them. This file contains **test procedures and expected results**, not completed test evidence. Record the actual observations after running the commands in GNS3.

## Test environment

- Router: `HQ-ROUTER-01` (Ubuntu Server, single 802.1Q trunk)
- Core: `HQ-CORE-SW1` (GNS3 built-in Ethernet Switch)
- Access switches: `HQ-ACCESS-SW1`, `HQ-ACCESS-SW2`
- Switch/router link: core Ethernet3 (Dot1Q, native VLAN 1) to router Ethernet0
- Host addressing and default gateways: see [router-configuration.md](router-configuration.md)

## Prerequisites

- [ ] Ubuntu shows `ens3.10`, `ens3.20`, `ens3.30`, `ens3.40`, `ens3.50`, and `ens3.99` with the expected gateway addresses (or equivalent NIC names).
- [ ] `sysctl net.ipv4.ip_forward` prints `net.ipv4.ip_forward = 1`.
- [ ] `ip route` shows six connected networks.
- [ ] Each VPCS `show ip` lists the correct address, subnet mask, and gateway.
- [ ] Ubuntu firewall forwarding policy allows the initial routing tests.

## A. Router-to-endpoint tests

Run these commands in the **Ubuntu router console**:

```bash
ping -c 4 192.168.10.10   # IT-PC1
ping -c 4 192.168.10.11   # IT-PC2
ping -c 4 192.168.20.10   # HR-PC1
ping -c 4 192.168.30.10   # FIN-PC1
ping -c 4 192.168.40.10   # ENG-PC1
ping -c 4 192.168.50.10   # SRV-01 (VPCS placeholder)
```

| ID | Source | Destination | Expected | Actual | Result |
|---|---|---|---|---|---|
| A1 | Ubuntu router | `192.168.10.10` | Replies received | Replies recieved | Passed |
| A2 | Ubuntu router | `192.168.10.11` | Replies received | Replies recieved | Passed |
| A3 | Ubuntu router | `192.168.20.10` | Replies received | Replies recieved | Passed |
| A4 | Ubuntu router | `192.168.30.10` | Replies received | Replies recieved | Passed |
| A5 | Ubuntu router | `192.168.40.10` | Replies received | Replies recieved | Passed |
| A6 | Ubuntu router | `192.168.50.10` | Replies received | Replies recieved | Passed |

## B. Inter-VLAN tests

Run the following commands in the specified **VPCS consoles**:

| ID | Source VPCS | Command | Expected | Actual | Result |
|---|---|---|---|---|---|
| B1 | IT-PC1 | `ping 192.168.20.10` | Success: IT → HR via router | Success | Passed |
| B2 | FIN-PC1 | `ping 192.168.50.10` | Success: Finance → Server | Success | Passed |
| B3 | ENG-PC1 | `ping 192.168.20.10` | Success: Engineering → HR | Success | Passed |
| B4 | IT-PC1 | `ping 192.168.10.11` | Success: IT → IT, same VLAN | Success | Passed |
| B5 | IT-PC1 | `ping 192.168.10.1` | Success: IT default gateway | Success | Passed |
| B6 | HR-PC1 | `ping 192.168.20.1` | Success: HR default gateway | Success | Passed |

> These cross-VLAN tests should **succeed before departmental firewall rules are added**. Later security phases will intentionally deny some traffic.

## C. Path verification

In the **IT-PC1 VPCS console**:

```text
trace 192.168.20.10
```

Expected: the first Layer 3 hop is the Ubuntu gateway at `192.168.10.1`, then the destination (subject to ICMP behavior).

**Actual traceroute output:** 

```text
trace to 192.168.20.10, 8 hops max, press Ctrl+C to stop
 1   192.168.10.1   0.425 ms  0.314 ms  0.410 ms
 2   *192.168.20.10   0.981 ms (ICMP type:3, code:3, Destination port unreachable)

```

## D. Wireshark validation

1. Packet capture data stored in inter-vlan-routing.pcapng

## E. Restart persistence test

1. Reboot `HQ-ROUTER-01`.
2. Confirm all six VLAN interfaces reappear using `ip -br addr`.
3. Confirm `sysctl net.ipv4.ip_forward` still returns `1`.
4. Repeat tests B1 and B2.

**results:** Success


## Summary

**Overall status:** Functional

**Date tested:** 11/17/26

**Notes / issues found:** N/A


