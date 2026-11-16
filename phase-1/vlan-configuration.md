# Phase 1 — VLAN and Switch Configuration

## Purpose

Segment the headquarters network into logical departments using **GNS3 built-in Ethernet Switch** nodes. The built-in switches are configured in GNS3's graphical interface, not through IOS commands.

## Switch inventory

| GNS3 node | Function |
| --- | --- |
| `HQ-CORE-SW1` | Connects both access switches and the server placeholder |
| `HQ-ACCESS-SW1` | IT and HR endpoints |
| `HQ-ACCESS-SW2` | IT, Finance, and Engineering endpoints |

## Logical VLANs

| VLAN | Name | Purpose |
| --- | --- | --- |
| 10 | IT | IT workstations, including two hosts across different access switches |
| 20 | HR | HR workstations |
| 30 | Finance | Finance workstations |
| 40 | Engineering | Engineering workstations |
| 50 | Servers | Simulated server segment |
| 99 | Management | Reserved for later management-plane design; no active endpoint |


## Physical connections

| From | From port | To | To port |
| --- | --- | --- | --- |
| `HQ-CORE-SW1` | Ethernet0 | `HQ-ACCESS-SW1` | Ethernet0 |
| `HQ-CORE-SW1` | Ethernet1 | `HQ-ACCESS-SW2` | Ethernet0 |
| `HQ-CORE-SW1` | Ethernet2 | `SRV-01` | Ethernet0 |
| `HQ-ACCESS-SW1` | Ethernet1 | `IT-PC1` | Ethernet0 |
| `HQ-ACCESS-SW1` | Ethernet2 | `HR-PC1` | Ethernet0 |
| `HQ-ACCESS-SW2` | Ethernet1 | `IT-PC2` | Ethernet0 |
| `HQ-ACCESS-SW2` | Ethernet2 | `FIN-PC1` | Ethernet0 |
| `HQ-ACCESS-SW2` | Ethernet3 | `ENG-PC1` | Ethernet0 |


### HQ-CORE-SW1

| Port | Type | VLAN field | Purpose |
| --- | --- | --- | --- |
| Ethernet0 | Dot1Q | 1 | Trunk to `HQ-ACCESS-SW1` |
| Ethernet1 | Dot1Q | 1 | Trunk to `HQ-ACCESS-SW2` |
| Ethernet2 | Access | 50 | `SRV-01` |
| Ethernet3–7 | Access | 1 | Unused/default ports |

### HQ-ACCESS-SW1

| Port | Type | VLAN field | Purpose |
| --- | --- | --- | --- |
| Ethernet0 | Dot1Q | 1 | Trunk to core |
| Ethernet1 | Access | 10 | `IT-PC1` |
| Ethernet2 | Access | 20 | `HR-PC1` |
| Ethernet3–7 | Access | 1 | Unused/default ports |

### HQ-ACCESS-SW2

| Port | Type | VLAN field | Purpose |
| --- | --- | --- | --- |
| Ethernet0 | Dot1Q | 1 | Trunk to core |
| Ethernet1 | Access | 10 | `IT-PC2` |
| Ethernet2 | Access | 30 | `FIN-PC1` |
| Ethernet3 | Access | 40 | `ENG-PC1` |
| Ethernet4–7 | Access | 1 | Unused/default ports |

## How to interpret Dot1Q mode

- **Access, VLAN 10**: Ingress untagged frames from that endpoint are classified into VLAN 10; tagged membership is handled by the switch.
- **Dot1Q, VLAN 1**: The port operates as an 802.1Q trunk; the VLAN field indicates the native/untagged VLAN (1), **not** that it only carries VLAN 1. Frames for VLANs such as VLAN 10 are tagged on the trunk.
- Keep the native VLAN consistent on both ends of each trunk.

The built-in Ethernet Switch is suitable for the basic VLAN behavior shown here. It is not a substitute for an enterprise switch's advanced STP, EtherChannel, port-security, or switch-management CLI features.

## Verification

1. Open each switch's GNS3 configuration and compare all port assignments to the tables above.
2. Start the VPCS nodes and check their IP settings using `show ip`.
3. From `IT-PC1`, ping `192.168.10.11` (`IT-PC2`) — should succeed through both trunks.
4. From `IT-PC1`, ping `192.168.20.10` (`HR-PC1`) — should fail without inter-VLAN routing.
5. Right-click the trunk between core and Access SW1 → **Start capture**; in Wireshark, display `vlan.id == 10` while generating IT-to-IT traffic. A trunk capture should show VLAN 10 tags, depending on capture interface behavior.



## Scope boundary

**Phase 1:** Layer 2 segmenting and trunking only. No operational gateway, DHCP, routing, ACLs, OSPF, VPN, or actual server applications have been configured.
