# CMPG 325 Computer Networks Project

## Boikarabelo Primary School Network Design

**Module:** CMPG 325 – Computer Networks  
**Project Type:** Individual Semester Project  
**Client:** Boikarabelo Primary School (Potchefstroom)  
**Client ID:** CLI-024  
**Industry:** Education  

---

## 1. Project Overview

This project involves the design and later implementation of a computer network for Boikarabelo Primary School in Potchefstroom.

The solution addresses the client's connectivity requirements, IPv4 addressing, IPv6 dual-stack addressing and routing, remote network-device management, and Guest Wi-Fi isolation.

The network is designed for implementation and testing in Cisco Packet Tracer.

---

## 2. Client Requirements

The project is based on the requirements in the CMPG 325 project brief:

- Assigned IPv4 block: `172.30.4.0/23`
- IPv6 dual-stack addressing and routing
- Remote management of network devices for an off-site IT contractor
- Guest Wi-Fi for visitors, isolated from internal resources
- A working and testable Packet Tracer implementation

---

## 3. Network Segmentation

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | Admin | Administration users |
| 20 | Staff | Teachers and staff |
| 30 | Lab | Student computer laboratory |
| 40 | Library | Library/media computers |
| 50 | Guest | Visitor Wi-Fi |
| 99 | Management | Network-device management |

---

## 4. Proposed Network Design

The proposed physical design contains:

- 1 edge router (R1)
- 1 core switch (SW1)
- 4 access switches (SW2–SW5)
- 1 wireless access point (AP1)
- Representative end-user devices

The four access switches provide dedicated access for Admin, Staff, Lab, and Library areas. The wireless access point provides the Guest network.

R1 provides inter-VLAN routing for IPv4 and IPv6 in the proposed design.

---

## 5. IPv4 Addressing

The assigned IPv4 block is:

`172.30.4.0/23`

VLSM is used to allocate separate networks for the VLANs. See `docs/ip-addressing-plan.md` for the complete table.

---

## 6. IPv6 Dual-Stack Design

IPv6 dual-stack addressing and routing is the assigned networking challenge.

For Milestone 1, the proposed IPv6 parent documentation prefix is:

`2001:db8:ac30:0::/60`

Each VLAN is allocated a `/64`:

- VLAN 10: `2001:db8:ac30:1::/64`
- VLAN 20: `2001:db8:ac30:2::/64`
- VLAN 30: `2001:db8:ac30:3::/64`
- VLAN 40: `2001:db8:ac30:4::/64`
- VLAN 50: `2001:db8:ac30:5::/64`
- VLAN 99: `2001:db8:ac30:6::/64`

This is a proposed documentation address plan, not an IPv6 block explicitly supplied in the project brief.

---

## 7. Remote Management

VLAN 99 is reserved for network-device management.

The final implementation will use SSH for secure remote management.

---

## 8. Guest Wi-Fi Isolation

VLAN 50 is dedicated to Guest Wi-Fi.

The final implementation will use access-control rules to prevent Guest users from accessing internal school resources while permitting the required external connectivity.

---

## 9. Milestone 1 Evidence

Milestone 1 focuses on:

- Client requirements
- Physical topology
- Logical topology
- IP addressing plan
- Initial GitHub repository

The Packet Tracer implementation and testing evidence will be added during Milestone 2.

---

## 10. Repository Structure

```text
boikarabelo-primary-network/
├── README.md
├── docs/
│   ├── client-requirements.md
│   ├── network-design.md
│   └── ip-addressing-plan.md
└── diagrams/
    ├── physical-topology.png
    └── logical-topology.png
```

## Academic Project Notice

This repository contains work completed as part of an academic project for CMPG 325 – Computer Networks.
