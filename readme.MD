# CCNA Networking Labs

This repository contains my **Cisco CCNA Packet Tracer labs** based on the learning path from **Jeremy's IT Lab CCNA course**.

Instead of creating completely separate labs for each topic, the **network topology evolves as new technologies are introduced**. Each lab builds on the previous one, gradually transforming the network into a more complex environment.

---

# Lab Design Workflow

The labs follow a **structured configuration workflow**, similar to how a network would be deployed in real environments.  
Each step builds on the previous configuration and introduces a new networking concept.

---

## Setup Before Configuration

Before starting device configuration, plan the IP scheme with **VLSM (Variable Length Subnet Masking)**.  
This helps assign subnet sizes based on host requirements instead of wasting address space.

Example approach:

- Use larger subnets for user VLANs that need more hosts
- Use smaller subnets for management or point-to-point links
- Document each subnet before assigning addresses to devices

### Example IP Structure

To keep the labs consistent, use a simple addressing convention inside each subnet:

- `.1` for important hosts or test PCs
- `.254` for the default gateway / SVI / router interface
- Low addresses like `.2`, `.3`, `.4` for additional hosts, switches, or management IPs

Example:

- VLAN 10: `192.168.10.0/24`
- Host PC: `192.168.10.1`
- Another host: `192.168.10.2`
- Gateway: `192.168.10.254`

This makes troubleshooting easier because host and gateway addresses are predictable across all labs.

## VLANs and Subnets Used in This Lab

The topology uses three user VLANs and two point-to-point WAN subnets.

### VLAN Networks

- **VLAN 10 - HR**
  - Subnet: `192.168.10.0/24`
  - Default gateway: `192.168.10.254`
  - Example host: `192.168.10.1`

- **VLAN 20 - Sales**
  - Subnet: `192.168.20.0/24`
  - Default gateway: `192.168.20.254`
  - Example host: `192.168.20.1`

- **VLAN 30 - IT**
  - Subnet: `192.168.30.0/24`
  - Default gateway: `192.168.30.254`
  - Example host: `192.168.30.1`

### FHRP Gateway Addresses

The lab also uses FHRP for gateway redundancy between the multilayer switches.

- VLAN 10 virtual gateway: `192.168.10.254`
- VLAN 20 virtual gateway: `192.168.20.254`
- VLAN 30 virtual gateway: `192.168.30.254`

Example addressing pattern shown in the topology:

- SW3 main SVI: `192.168.x.252`
- SW3 backup SVI: `192.168.x.253`
- Router or FHRP virtual gateway: `192.168.x.254`

Replace `x` with the VLAN number, such as `10`, `20`, or `30`.

### Point-to-Point Routed Links

- **R3 to ISP**
  - Subnet: `203.0.113.0/30`
  - R3: `203.0.113.1`
  - ISP: `203.0.113.2`

- **R4 to ISP**
  - Subnet: `203.0.113.4/30`
  - R4: `203.0.113.5`
  - ISP: `203.0.113.6`

---

## Lab Configuration Steps

1. **Configure Hostnames on all devices**

   - Routers
   - Switches
   - Hosts

2. **Configure IP addressing on end hosts**

   - Assign IP address
   - Assign subnet mask
   - Configure default gateway

3. **Configure VLANs and VTP**

   - Create VLANs **10, 20, 30**
   - Configure **VTP between Switches**

4. **Configure Trunk Links**

   - Configure trunk ports between:
     - SW1
     - SW2
     - SW3

5. **Configure Routed Ports**

   - Configure **point-to-point routed link between:**
     - SW3 (main) and R3
     - SW3 (backup) and R4
     - R3 and ISP
     - R4 and ISP

6. **Configure Spanning Tree (RSTP)**

   - Enable **RSTP**
   - Configure **root bridge**

7. **Configure PortFast and BPDU Guard**

   - Enable **PortFast on access ports**
   - Enable **BPDU Guard for edge protection**

8. **Configure SVIs**

   - Create **SVIs on SW3**
   - Provide **inter-VLAN routing**

9. **Verify VLAN Connectivity**

   - Test **ping between VLANs**
   - Verify **hosts reach their gateways**

10. **Configure Router Interfaces**

    - Configure IP addressing on router interfaces

11. **Configure OSPF**

    - Configure OSPF on:
      - Routers
      - SW3 (Layer 3 switch)
    - Configure router id's, loopbacks, passive-interfaces
    - Enable via network command (wildcard mask) for (SVIs) and per interface 

12. **Verify OSPF Adjacency**

    - Verify **OSPF neighbors**
    - Verify **route exchange**

13. **Verify End-to-End Connectivity**

    - Routers reach VLAN networks
    - End hosts reach routers
    - SVIs reachable from routers

14. **Configure EtherChannel**

    - Configure **EtherChannel between SW1 and SW2 to SW3**
    - Increase bandwidth and redundancy

15. **Configure FHRP**

    - Configure **First Hop Redundancy Protocol**
    - Provide **default gateway redundancy for VLANs**

---

This lab will continue to grow as I progress to Jeremy's IT Labs CCNA Lectures.

**Sample Lab**
![Sample lab image](sample-lab.png)