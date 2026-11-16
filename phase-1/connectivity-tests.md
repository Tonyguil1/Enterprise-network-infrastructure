# Phase 1 — Connectivity and VLAN Isolation Tests

## Objective

Validate Layer 2 VLAN segmentation and 802.1Q trunking across the headquarters network in GNS3.

## Lab setup

- Switches: `HQ-CORE-SW1`, `HQ-ACCESS-SW1`, `HQ-ACCESS-SW2` (GNS3 built-in Ethernet Switch)
- Nodes: `IT-PC1`, `IT-PC2`, `HR-PC1`, `FIN-PC1`, `ENG-PC1`, `SRV-01` (VPCS)
- VLAN IDs: 10, 20, 30, 40, 50, 99 (management reserved)
- Routing: **not enabled**; gateway IPs are reserved but do not exist yet
- Date tested: `2026-11-16`
- GNS3 version: `2.2.55`
- Host environment: `linux`

## Test matrix

| Test ID | Source | Destination | Command issued at source | Expected | Actual result | Pass/Fail |
| --- | --- | --- | --- | --- | --- | --- |
| T01 | `IT-PC1` | `IT-PC2` (`192.168.10.11`) | `ping 192.168.10.11` | Success: same VLAN across switches | Success | Pass |
| T02 | `IT-PC2` | `IT-PC1` (`192.168.10.10`) | `ping 192.168.10.10` | Success: reverse direction | Success | Pass |
| T03 | `IT-PC1` | `HR-PC1` (`192.168.20.10`) | `ping 192.168.20.10` | Failure: no inter-VLAN route | Failure | Pass |
| T04 | `IT-PC2` | `FIN-PC1` (`192.168.30.10`) | `ping 192.168.30.10` | Failure: no inter-VLAN route | Failure | Pass |
| T05 | `ENG-PC1` | `SRV-01` (`192.168.50.10`) | `ping 192.168.50.10` | Failure: no inter-VLAN route | Failure | Pass |
| T06 | `HR-PC1` | `IT-PC1` (`192.168.10.10`) | `ping 192.168.10.10` | Failure: no inter-VLAN route | Failure | Pass |

> A failed cross-VLAN ping here is **expected because no Layer 3 gateway exists**, not proof that a firewall or ACL blocked the traffic. Some VPCS versions may report an unreachable gateway instead of a simple timeout; either is consistent with the expected lack of routing.

## Test procedure

### 1. Validate endpoint addressing

On each VPCS console:

```text
show ip
```

Confirm the address and `/24` mask match [`ip-addressing.md`](ip-addressing.md). Check for typos and duplicate IP addresses.

- Result: `Success`
- Notes / evidence: `No duplicates and matching ip addresses`

### 2. Validate the switch port configuration

Open each built-in switch's GNS3 **Configure → Ethernet switch** dialog and confirm the port assignments in [`vlan-configuration.md`](vlan-configuration.md).

- Trunks: core `Ethernet0` and `Ethernet1`, access switch `Ethernet0`, all `Dot1Q / VLAN 1`
- Access ports: VLAN 10, 20, 30, 40, or 50 as specified
- Result: `Success`
- Evidence screenshot path: `enterprise-network-lab/phase-1/topology.png`

### 3. Test same-VLAN connectivity

Open `IT-PC1` console:

```text
ping 192.168.10.11
```

**Expected:** ICMP echo replies from `IT-PC2`. The traffic crosses Access SW1 → Core → Access SW2 while remaining in VLAN 10.

- Actual packet loss / response: `84 bytes from 192.168.10.11 icmp_seq=1 ttl=64 time =0.407 ms`
- Result (Pass/Fail): `Pass`


If the first attempt fails while links initialize, repeat the ping. Persistent failure means further investigation is needed.

### 4. Test the absence of inter-VLAN routing

Open `IT-PC1` console:

```text
ping 192.168.20.10
```

**Expected:** No ICMP echo reply. IT and HR are in different VLANs and IP subnets; no active router has been configured.

- Actual output: `host (192.168.10.1) not reachable`
- Result (Pass/Fail): `Pass`


Repeat the other cross-VLAN cases from the test matrix.

### 5. Capture VLAN-tagged traffic in Wireshark

1. In GNS3, right-click the link connecting `HQ-CORE-SW1 Ethernet0` and `HQ-ACCESS-SW1 Ethernet0`.
2. Select **Start capture** and open Wireshark.
3. From `IT-PC1`, run `ping 192.168.10.11`.
4. Apply the display filter:

```text
vlan.id == 10
```

5. Inspect a packet for an 802.1Q header with VLAN ID 10. If no packets appear, clear the display filter and check capture link selection, packet encapsulation, and traffic generation.
6. Save the capture as `vlan10-trunk.pcapng` inside this Phase 1 directory.

- VLAN 10 tag observed? `Yes`
- Capture filename: `vlan10-trunk.pcapg`


