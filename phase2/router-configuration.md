# Phase 2 — Ubuntu Router Configuration

## Objective

Implement **inter-VLAN routing (router-on-a-stick)** in the GNS3 headquarters network using an Ubuntu Server VM and the GNS3 built-in Ethernet switches. This phase adds routing between the VLANs created in Phase 1; security restrictions will be configured in a later phase.

## Topology changes

- Add a QEMU/KVM Ubuntu Server VM named `HQ-ROUTER-01`
- Connect **`HQ-CORE-SW1 Ethernet3`** to **`HQ-ROUTER-01 Ethernet0`**.
- On the GNS3 built-in core switch, configure **Ethernet3** as **Dot1Q** with **native VLAN 1**.
- Keep the two existing core-to-access-switch trunks and all existing PC connections unchanged.
- The Ubuntu router NIC is called **`ens3` in the examples below**. Verify the actual name with `ip -br link`, and replace `ens3` if necessary.

> **Note:** The built-in Ethernet Switch uses its graphical port configuration, not Cisco IOS CLI commands. A Dot1Q port's VLAN setting is the *native/untagged* VLAN; other VLANs are carried with 802.1Q tags.

## Gateway addressing

| VLAN | Network | Ubuntu VLAN interface | Gateway address |
|---|---|---|---|
| 10 — IT | `192.168.10.0/24` | `ens3.10` | `192.168.10.1/24` |
| 20 — HR | `192.168.20.0/24` | `ens3.20` | `192.168.20.1/24` |
| 30 — Finance | `192.168.30.0/24` | `ens3.30` | `192.168.30.1/24` |
| 40 — Engineering | `192.168.40.0/24` | `ens3.40` | `192.168.40.1/24` |
| 50 — Servers | `192.168.50.0/24` | `ens3.50` | `192.168.50.1/24` |
| 99 — Management | `192.168.99.0/24` | `ens3.99` | `192.168.99.1/24` |



