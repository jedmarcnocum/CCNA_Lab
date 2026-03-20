# CCNA Networking Lab Portfolio

A growing collection of Cisco Packet Tracer labs documenting my hands-on CCNA practice in switching, routing, redundancy, and IPv6.

![Sample lab topology](sample-lab.png)

## Overview

This repository serves as my **networking portfolio** while I work through my CCNA course and build practical configuration skills in Packet Tracer.

Instead of treating each topic as a completely separate project, I use an **evolving topology** that becomes more advanced over time. That approach helps me practice how networks are built, expanded, verified, and troubleshot.

## What This Repo Shows

- VLAN creation and segmentation
- VTP and trunking between switches
- Inter-VLAN routing with SVIs and Layer 3 switching
- RSTP, PortFast, BPDU Guard, and Root Guard
- EtherChannel for redundancy and bandwidth
- OSPF routing and failover path behavior
- FHRP for resilient default gateways
- IPv4 and IPv6 dual-stack addressing
- Static routing and WAN edge connectivity
- Standard ACL policy enforcement for segmented access control

## Lab Files

These Packet Tracer labs show the progression of the topology and the topics covered:

| Lab File | Main Focus |
|---|---|
| `VTP.pkt` | VLAN propagation and switch domain setup |
| `VLAN - L2 Switch (ROAS) + Subinterfaces.pkt` | Router-on-a-stick and inter-VLAN routing |
| `VLAN - L3 Switch (P2P) + SVI.pkt` | Layer 3 switching with SVIs and routed links |
| `RSTP with portfast, bpduguard, root guard.pkt` | Spanning-tree protection features |
| `Etherchannel.pkt` | Aggregated uplinks and redundancy |
| `FHRP (Active, Standby).pkt` | Default gateway redundancy |
| `OSPF (cost changed going to backup router).pkt` | Dynamic routing and path preference |
| `IPv6.pkt` | Dual-stack addressing with IPv6 static routes |

## Lab Exercise: Configure Standard ACLs

This exercise uses the same addressing plan already defined in this repository.

### Objective

Allow only the **HR VLAN (192.168.10.0/24)** to access a protected internal management endpoint in **VLAN 30 (IT)** while denying source traffic from Sales and other user VLANs.

### Scenario

- Protected destination host: `192.168.30.10` (example server in IT VLAN)
- Allowed source subnet: `192.168.10.0/24` (HR)
- Blocked source subnet: `192.168.20.0/24` (Sales)
- Device for ACL placement: multilayer switch SVI for VLAN 30, applied **outbound** (closest to destination)

### Configuration Tasks

1. Create a numbered standard ACL that permits HR and denies all other sources.
2. Apply the ACL to the VLAN 30 SVI in the outbound direction.
3. Save configuration and verify ACL hit counters.
4. Validate results with pings from HR and Sales test hosts to `192.168.30.10`.

### Example Cisco IOS Configuration

```cisco
! On the multilayer switch performing inter-VLAN routing
conf t
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any
!
interface vlan 30
 ip access-group 10 out
end
wr
```

### Verification Commands

```cisco
show access-lists
show ip interface vlan 30
show run | section access-list
```

### Expected Results

- HR host (for example `192.168.10.1`) can reach `192.168.30.10`.
- Sales host (for example `192.168.20.1`) is denied access to `192.168.30.10`.
- ACL counters increment on both permit and deny statements after testing.

## Topology Snapshot

The current lab design includes:

- Three user VLANs: HR, Sales, and IT
- Two distribution / multilayer switches acting as primary and backup gateways
- Redundant uplinks between access and distribution layers
- Dual upstream routers connected to an ISP
- IPv4 and IPv6 addressing throughout the environment

## Addressing Plan

### VLAN Networks

| VLAN | Department | IPv4 Subnet | IPv4 Gateway | IPv6 Subnet | IPv6 Gateway |
|---|---|---|---|---|---|
| 10 | HR | `192.168.10.0/24` | `192.168.10.254` | `2001:DB8:10::/64` | `2001:DB8:10::254` |
| 20 | Sales | `192.168.20.0/24` | `192.168.20.254` | `2001:DB8:20::/64` | `2001:DB8:20::254` |
| 30 | IT | `192.168.30.0/24` | `192.168.30.254` | `2001:DB8:30::/64` | `2001:DB8:30::254` |

### Example End-Host Addressing

| VLAN | Example IPv4 Host | Example IPv6 Host |
|---|---|---|
| 10 | `192.168.10.1` | `2001:DB8:10::1` |
| 20 | `192.168.20.1` | `2001:DB8:20::1` |
| 30 | `192.168.30.1` | `2001:DB8:30::1` |

### FHRP / Gateway Pattern

The multilayer switches follow a predictable addressing structure to make verification and troubleshooting easier.

| Role | IPv4 Pattern | IPv6 Pattern |
|---|---|---|
| Main multilayer switch SVI | `192.168.x.252` | `2001:DB8:x::252` |
| Backup multilayer switch SVI | `192.168.x.253` | `2001:DB8:x::253` |
| Virtual default gateway | `192.168.x.254` | `2001:DB8:x::254` |

Replace `x` with the VLAN number such as `10`, `20`, or `30`.

### Point-to-Point WAN Links

| Link | IPv4 Subnet | Router Address | ISP Address | IPv6 Subnet | Router IPv6 | ISP IPv6 |
|---|---|---|---|---|---|---|
| R3 to ISP | `203.0.113.0/30` | `203.0.113.1` | `203.0.113.2` | `2001:DB8:113:1::/64` | `2001:DB8:113:1::1` | `2001:DB8:113:1::2` |
| R4 to ISP | `203.0.113.4/30` | `203.0.113.5` | `203.0.113.6` | `2001:DB8:113:2::/64` | `2001:DB8:113:2::1` | `2001:DB8:113:2::2` |

## Configuration Workflow

This is the general sequence I follow as the topology evolves:

1. Configure hostnames on routers, switches, and hosts.
2. Assign IPv4 and IPv6 addressing to end devices.
3. Create VLANs and configure VTP where required.
4. Build trunk links between switches.
5. Configure routed ports and WAN point-to-point links.
6. Enable RSTP and harden the edge with PortFast and BPDU Guard.
7. Create SVIs for inter-VLAN routing.
8. Verify VLAN reachability and gateway connectivity.
9. Configure router interfaces and upstream connectivity.
10. Deploy OSPF and validate adjacency plus route exchange.
11. Add EtherChannel for redundancy and higher throughput.
12. Configure FHRP for resilient default gateway services.
13. Apply ACL policy controls and validate source-based filtering behavior.
14. Test failover, path selection, and end-to-end connectivity.

## Design Notes
- The topology is intentionally **progressive**, so each lab builds on earlier concepts instead of starting from zero.
- I use **VLSM planning and consistent gateway conventions** to keep addressing easy to read.
 I document subnetting in a step-by-step format before assigning addresses. This strengthens my **subnetting skills** by forcing me to calculate host requirements, masks, usable ranges, broadcasts, and the next available network instead of guessing.
- I include both **IPv4 and IPv6** to strengthen dual-stack configuration and troubleshooting skills.
- Redundancy features such as **FHRP, EtherChannel, and spanning-tree protections** are included to reflect real design patterns.

### Example VLSM Workflow

The same structured approach is used when building subnets for the lab. In this topology, the user VLANs are intentionally kept as `/24` networks for simplicity and room to grow, while WAN point-to-point links use `/30` subnets:

```text
HR VLAN 10
Host bits: 2^8 = 256
Mask: 255.255.255.0
Network address: 192.168.10.0/24
Prefix: /24
Usable range: 192.168.10.1 - 192.168.10.254
Broadcast address: 192.168.10.255
Gateway: 192.168.10.254

Sales VLAN 20
Host bits: 2^8 = 256
Mask: 255.255.255.0
Network address: 192.168.20.0/24
Prefix: /24
Usable range: 192.168.20.1 - 192.168.20.254
Broadcast address: 192.168.20.255
Gateway: 192.168.20.254

IT VLAN 30
Host bits: 2^8 = 256
Mask: 255.255.255.0
Network address: 192.168.30.0/24
Prefix: /24
Usable range: 192.168.30.1 - 192.168.30.254
Broadcast address: 192.168.30.255
Gateway: 192.168.30.254

R3-ISP P2P
Host bits: 2^2 = 4
Mask: 255.255.255.252
Network address: 203.0.113.0/30
Prefix: /30
Usable range: 203.0.113.1 - 203.0.113.2
Broadcast address: 203.0.113.3

R4-ISP P2P
Host bits: 2^2 = 4
Mask: 255.255.255.252
Network address: 203.0.113.4/30
Prefix: /30
Usable range: 203.0.113.5 - 203.0.113.6
Broadcast address: 203.0.113.7
```

## Next Steps

As this portfolio grows, I plan to add:

- More annotated screenshots
- Per-lab notes and verification outputs
- Troubleshooting scenarios and recovery steps
- Additional routing, security, and services labs as I progress to my CCNA lectures

---
