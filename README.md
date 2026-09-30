# CMPG 325 Computer Networks Project

## Boikarabelo Primary School Network Design and Implementation

**Module:** CMPG 325 – Computer Networks
**Project Type:** Individual Semester Project
**Project ID:** CMPG325-2026-024
**Client:** Boikarabelo Primary School (Potchefstroom)
**Client ID:** CLI-024
**Industry:** Education
**Author:** Sesethu K

---

## 1. Project Overview

This project involves the design and implementation of a computer network for **Boikarabelo Primary School (Potchefstroom)**.

The network was designed according to the CMPG 325 project requirements and implemented and tested using Cisco Packet Tracer.

The solution provides:

* IPv4 addressing using the assigned `172.30.4.0/23` block
* IPv4 VLSM subnetting
* IPv6 dual-stack addressing and routing
* VLAN-based network segmentation
* Inter-VLAN routing
* Wired network connectivity
* Guest Wi-Fi
* Guest network isolation from internal school resources
* Dedicated network-device management
* SSH remote management
* IPv4 and IPv6 connectivity testing

---

## 2. Client Requirements

The project requires a network solution that provides appropriate connectivity for the school while separating different user groups.

The main requirements addressed by this implementation are:

* Use of the assigned IPv4 block `172.30.4.0/23`
* Separate VLANs for the different school/user groups
* IPv4 and IPv6 dual-stack networking
* IPv6 subnet addressing and routing as the assigned networking challenge
* Remote management of network devices
* Guest Wi-Fi for visitors
* Isolation of Guest users from internal school resources
* A working and testable Cisco Packet Tracer implementation

The client requirements document identifies a dedicated Management VLAN and a separate Guest Wi-Fi network.

---

## 3. Network Segmentation

The network is divided into separate VLANs to provide logical separation between different user groups and network functions.

| VLAN | Name       | Purpose                     |
| ---- | ---------- | --------------------------- |
| 10   | Admin      | Administration users        |
| 20   | Staff      | Teachers and staff          |
| 30   | Lab        | Student computer laboratory |
| 40   | Library    | Library/media computers     |
| 50   | Guest      | Visitor Wi-Fi               |
| 99   | Management | Network-device management   |

This segmentation supports the requirement to separate user groups and isolate Guest users from internal school resources.

---

## 4. Physical Network Design

The implemented topology consists of:

* **R1** – Router and inter-VLAN routing
* **SW1** – Core switch
* **SW2** – Admin access switch
* **SW3** – Staff access switch
* **SW4** – Lab access switch
* **SW5** – Library and Guest access switch
* **AP1** – Guest wireless access point
* Representative wired end-user devices
* Representative Guest wireless device

The main topology is:

```text
                         R1
                         |
                    802.1Q Trunk
                         |
                        SW1
              ___________|____________
             |        |       |       |
            SW2      SW3     SW4     SW5
             |        |       |      /  \
           Admin    Staff    Lab    /    \
                                  /        \
                           PC-LIBRARY       AP1
                                             |
                                      ~~~ Wi-Fi ~~~
                                             |
                                      GUEST-DEVICE
```

R1 uses router-on-a-stick inter-VLAN routing. The connection between R1 and SW1 carries the VLAN traffic using 802.1Q trunking.

SW1 provides the core switching function and connects the access switches.

SW5 provides wired connectivity for the Library network and a separate connection to AP1 for the Guest Wi-Fi network.

---

## 5. IPv4 Addressing

The assigned IPv4 address block is:

```text
172.30.4.0/23
```

VLSM was used to divide the address block into separate networks for the VLANs.

| VLAN          | Network        | Prefix | Gateway        |
| ------------- | -------------- | ------ | -------------- |
| 30 Lab        | `172.30.4.0`   | `/26`  | `172.30.4.1`   |
| 20 Staff      | `172.30.4.64`  | `/26`  | `172.30.4.65`  |
| 50 Guest      | `172.30.4.128` | `/26`  | `172.30.4.129` |
| 10 Admin      | `172.30.4.192` | `/27`  | `172.30.4.193` |
| 40 Library    | `172.30.4.224` | `/27`  | `172.30.4.225` |
| 99 Management | `172.30.5.0`   | `/28`  | `172.30.5.1`   |

The WAN network is:

```text
172.30.5.16/30
```

with:

* R1: `172.30.5.17`
* ISP: `172.30.5.18`

---

## 6. IPv6 Dual-Stack Design

IPv6 dual-stack addressing and routing is the assigned networking challenge.

The assigned IPv6 block is:

```text
2001:DB8:AC30::/48
```

A `/60` sub-block was selected for the network design:

```text
2001:DB8:AC30:0::/60
```

This `/60` is a design choice within the assigned `/48`. It is divided into `/64` networks for the VLANs.

| VLAN          | IPv6 Network           | Gateway              |
| ------------- | ---------------------- | -------------------- |
| 10 Admin      | `2001:DB8:AC30:1::/64` | `2001:DB8:AC30:1::1` |
| 20 Staff      | `2001:DB8:AC30:2::/64` | `2001:DB8:AC30:2::1` |
| 30 Lab        | `2001:DB8:AC30:3::/64` | `2001:DB8:AC30:3::1` |
| 40 Library    | `2001:DB8:AC30:4::/64` | `2001:DB8:AC30:4::1` |
| 50 Guest      | `2001:DB8:AC30:5::/64` | `2001:DB8:AC30:5::1` |
| 99 Management | `2001:DB8:AC30:6::/64` | `2001:DB8:AC30:6::1` |

IPv6 forwarding is enabled on R1 using:

```text
ipv6 unicast-routing
```

The IPv6 routing table was verified using:

```text
show ipv6 route
```

The VLAN IPv6 networks appear as directly connected routes on R1.

---

## 7. Inter-VLAN Routing

R1 performs inter-VLAN routing using router-on-a-stick.

The configured R1 subinterfaces are:

```text
Gi0/0.10
Gi0/0.20
Gi0/0.30
Gi0/0.40
Gi0/0.50
Gi0/0.99
```

Each subinterface uses 802.1Q encapsulation for its corresponding VLAN.

For example:

```text
interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 172.30.4.193 255.255.255.224
```

The same approach is used for the remaining VLANs.

---

## 8.Guest Wi-Fi and Isolation

VLAN 50 is dedicated to Guest Wi-Fi to accommodate visitors while keeping Guest traffic separate from internal school resources.

Guest isolation is implemented using access-control lists on R1:

* `GUEST_ISOLATION` controls Guest IPv4 traffic.
* `GUEST_V6_ISOLATION` controls Guest IPv6 traffic.

The Guest network is prevented from accessing the Admin, Staff, Lab, Library and Management networks over both IPv4 and IPv6.

Guest connectivity was tested using a temporary wireless test laptop. The tests confirmed that the Guest device could reach its own gateway while access to the internal school networks was blocked.

The Guest test device is used for testing evidence only and is not part of the permanent school network design.

---

## 9. Remote Network Management

VLAN 99 is dedicated to network-device management.

| Device | Management IP |
| ------ | ------------- |
| R1     | `172.30.5.1`  |
| SW2    | `172.30.5.2`  |
| SW3    | `172.30.5.3`  |
| SW4    | `172.30.5.4`  |
| SW5    | `172.30.5.5`  |

SSH was configured for secure remote management.

The VTY configuration includes:

```text
line vty 0 4
 login local
 transport input ssh
```

SSH was tested from PC-ADMIN to the network devices, including R1 and the access switches.

---

## 10. Testing and Verification

Testing was performed in Cisco Packet Tracer to verify VLAN configuration, IPv4 and IPv6 connectivity, Guest Wi-Fi isolation and SSH remote management.

### IPv4 Connectivity

Successful IPv4 connectivity was verified between:

* Admin → Staff
* Admin → Lab
* Admin → Library

Evidence:

* `evidence/ipv4/admin-to-staff-ipv4.png`
* `evidence/ipv4/admin-to-lab-ipv4.png`
* `evidence/ipv4/admin-to-library-ipv4.png`

### IPv6 Connectivity

Successful IPv6 connectivity was verified between:

* Admin → Staff
* Lab → Admin

The IPv6 routing table on R1 was also checked and showed the configured VLAN networks as connected routes.

Evidence:

* `evidence/ipv6/admin-to-staff-ipv6.png`
* `evidence/ipv6/lab-to-admin-ipv6.png`
* `evidence/ipv6/ipv6-routing-table.png`

### Testing Summary

| Test                            | Result     |
| ------------------------------- | ---------- |
| Admin → Staff IPv4              | Successful |
| Admin → Lab IPv4                | Successful |
| Admin → Library IPv4            | Successful |
| Admin → Staff IPv6              | Successful |
| Lab → Admin IPv6                | Successful |
| IPv6 routing table verification | Successful |
| Guest → IPv4 Gateway            | Successful |
| Guest → Internal IPv4 Networks  | Blocked    |
| Guest → IPv6 Gateway            | Successful |
| Guest → Internal IPv6 Networks  | Blocked    |
| SSH → R1                        | Successful |
| SSH → SW2                       | Successful |
| SSH → SW3                       | Successful |
| SSH → SW4                       | Successful |
| SSH → SW5                       | Successful |

The testing results demonstrate IPv4 and IPv6 connectivity between required networks, Guest isolation from internal resources, IPv6 routing, and secure SSH remote management.


### SSH Remote Management

SSH was tested from PC-ADMIN to R1 and the switches. Successful SSH access was demonstrated for R1, SW2, SW3, SW4 and SW5.

Evidence:

* `evidence/ssh/ssh-configuration.png`
* `evidence/ssh/ssh-r1-test.png`

---

## 11. Assigned Networking Challenge

The assigned networking challenge is:

**IPv6 subnet addressing and routing**

The implementation addresses this challenge by:

1. Using the assigned IPv6 `/48` block.
2. Selecting a `/60` sub-block for the network design.
3. Creating separate `/64` networks for the VLANs.
4. Assigning IPv6 gateway addresses to the R1 VLAN subinterfaces.
5. Enabling IPv6 forwarding.
6. Verifying the IPv6 routing table.
7. Testing IPv6 connectivity between VLANs.

---

## 12. Implementation Documentation

Detailed implementation information is available in:

[`docs/implementation.md`](docs/implementation.md)

This document records the implemented devices, VLANs, IPv4 and IPv6 addressing, inter-VLAN routing, Guest Wi-Fi isolation and SSH configuration.

---

## 13. Evidence

Supporting implementation and testing evidence is organised into the following directories:

```text
evidence/
├── topology/
├── vlan/
├── ipv4/
├── ipv6/
├── security/
└── ssh/
```

The evidence includes screenshots covering:

* Network topology
* VLAN configuration
* IPv4 connectivity
* IPv6 connectivity
* IPv6 routing
* Guest isolation
* SSH configuration
* SSH connectivity

---

## 14. Repository Structure

```text
boikarabelo-primary-network/
│
├── README.md
│
├── CMPG325-2026-024_Milestone2.pkt
│
├── docs/
│   ├── client-requirements.md
│   ├── network-design.md
│   ├── ip-addressing-plan.md
│   ├── implementation.md
│   └── testing.md
│
├── diagrams/
│   ├── physical-topology.jpg
│   └── logical-topology.jpg
│
└── evidence/
    ├── topology/
    ├── vlan/
    ├── ipv4/
    ├── ipv6/
    ├── security/
    └── ssh/
```

---

## 15. Conclusion

The Milestone 2 implementation provides a segmented dual-stack network for Boikarabelo Primary School.

The implemented solution includes:

* VLAN-based network segmentation
* IPv4 VLSM addressing
* IPv6 `/64` subnet allocation
* Inter-VLAN routing
* Guest Wi-Fi
* Guest isolation using an ACL
* Dedicated network-device management
* SSH remote management
* IPv4 connectivity testing
* IPv6 connectivity testing
* IPv6 routing verification

The network was implemented and tested in Cisco Packet Tracer according to the CMPG 325 project requirements.

---

## Academic Integrity

This repository documents individual coursework for CMPG 325 (Project ID: CMPG325-2026-024).

Boikarabelo Primary School (Potchefstroom) is a client scenario assigned by the module and is not an active commercial engagement.

The implementation, configurations, testing and documentation represent the student's own coursework and learning process.

