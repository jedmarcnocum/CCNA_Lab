# CCNA Mini Megalab – Step-by-Step Build Notes

This document is a **CCNA-exam–oriented checklist** for building a simple but complete lab using:

* **1 Router (R1)**
* **1 Layer 3 Switch (SW2)**
* **1 Layer 2 Switch (SW1)**
* **VLANs + VLSM + Inter-VLAN Routing**

Packet Tracer may hide some requirements; this guide follows **real Cisco IOS logic**.

---

## Useful Verification Commands (Memorize These)

* **`show ip interface brief`**  
  Shows IP addresses and interface up/down state
* **`show interface status`**  
  Shows interface status, VLAN assignment, speed, duplex
* **`show interfaces trunk`**  
  Shows trunk ports and allowed VLANs
* **`show vlan brief`**  
  Shows VLANs, names, status, and access ports
* **`switchport trunk encapsulation dot1q`**  
  Required on **older Cisco switches** that support ISL and 802.1Q

---

## Step 0 – Network Planning (Do This First)

1. Compute **VLSM** based on:

   * Base network address
   * Required hosts per VLAN

2. For each VLAN, determine:

   * Network address
   * Usable range
   * Default gateway IP

> ⚠️ No device configuration should start until VLSM is complete.

---

## Step 1 – Lab Setup

1. Place devices in Packet Tracer
2. Add **straight-through cables**
3. Rename devices immediately:

   * **`hostname SW1`**
   * **`hostname SW2`**
   * **`hostname R1`**

---

## Step 2 – PC Host Configuration

For each PC:

1. Set **IP address** → *first usable address* of its VLAN subnet
2. Set **Subnet Mask** → based on VLSM calculation
3. Set **Default Gateway** → *last usable address* (SVI on SW2)

> PCs never know VLANs — only IP information.

---

## Step 3 – SW1 (Layer 2 Switch) Configuration

### 3.1 Create VLANs (Required on Real Cisco Devices)

```
vlan 10
vlan 20
vlan 30
```

> VLANs \*\*must exist before\*\* assigning ports.

---

### 3.2 Configure Trunk Toward SW2

```
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

---

### 3.3 Configure Access Ports for PCs

```
interface f0/1
switchport mode access
switchport access vlan 10
```

(Repeat for VLAN 20 / 30 as needed)

---

### 3.4 Verify

* **`show vlan brief`**
* **`show interfaces trunk`**
* **`show interface status`**

---

## Step 4 – SW2 (Layer 3 Switch) Configuration

### 4.1 Enable Layer 3 Routing (CRITICAL)

```
ip routing
```

Without this, the switch will NOT route between VLANs.

---

### 4.2 Create VLANs

```
vlan 10
vlan 20
vlan 30
```

---

### 4.3 Configure Trunk Toward SW1

```
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

---

### 4.4 Configure SVIs (Default Gateways)

```
interface vlan 10
ip address x.x.x.x y.y.y.y
no shutdown
```

(Repeat for VLAN 20 and VLAN 30)

---

### 4.5 Configure Routed Port Toward Router

```
interface g0/2
no switchport
ip address x.x.x.x y.y.y.y
no shutdown
```

> Routed ports \*\*cannot carry VLANs\*\*.

---

### 4.6 Configure Default Route (Gateway of Last Resort)

```
ip route 0.0.0.0 0.0.0.0 <R1-P2P-IP>
```

---

## Step 5 – R1 (Router) Configuration

### 5.1 Configure P2P Interface

```
interface g0/0
ip address x.x.x.x y.y.y.y
no shutdown
```

---

### 5.2 Add Static Routes to VLAN Networks

```
ip route <VLAN-NETWORK> <MASK> <SW2-P2P-IP>
```

(Repeat for VLAN 10, 20, and 30)

---

### 5.3 Configure Default Route to Internet Cloud

```
ip route 0.0.0.0 0.0.0.0 <ISP-NEXT-HOP>
```

---

## Step 6 – Final Verification Checklist

* **`show vlan brief`**
* **`show interfaces trunk`**
* **`show ip interface brief`**
* **`show ip route`**
* Ping tests:

  * Same VLAN
  * Different VLAN
  * PC → Router

---

## CCNA GOLD RULES

* VLANs must be **created before assignment**
* Trunks exist **only between switches**
* Routed ports use **`no switchport`**
* SVIs require:

  * VLAN existence
  * Trunk connectivity
  * **`ip routing` enabled**

* Routers do **not** use `ip default-gateway`

---

**End of CCNA Mini Megalab Notes**

