# Client Requirements

## 1. Client Information

**Client:** Boikarabelo Primary School  
**Location:** Potchefstroom  
**Client ID:** CLI-024  
**Industry:** Education

---

## 2. Network Requirements

### 2.1 IPv4 Addressing

The project brief assigns the IPv4 addressing block:

`172.30.4.0/23`

The block will be subnetted using VLSM to provide separate networks for the different functional areas.

### 2.2 IPv6 Dual-Stack

The assigned networking challenge is:

**IPv6 – Dual-stack addressing and routing**

The proposed network therefore supports both IPv4 and IPv6.

For Milestone 1, the proposed IPv6 documentation prefix is `2001:db8:ac30:0::/60`, divided into `/64` networks for the VLANs. This is a proposed design choice; the project brief shown does not explicitly supply an IPv6 block.

### 2.3 Remote Network Management

Remote management of network devices is required for the off-site IT contractor.

A dedicated Management VLAN (VLAN 99) is included in the proposed design.

### 2.4 Guest Wi-Fi

Guest Wi-Fi must be added for visitors and isolated from internal resources.

VLAN 50 is dedicated to Guest Wi-Fi, with access-control rules planned to prevent access to internal school networks.

---

## 3. Proposed Network Segmentation

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | Admin | Administration users |
| 20 | Staff | Teachers and staff |
| 30 | Lab | Student computer laboratory |
| 40 | Library | Library/media computers |
| 50 | Guest | Visitor Wi-Fi |
| 99 | Management | Network-device management |

---

## 4. Design Objectives

The proposed design aims to:

1. Provide appropriate connectivity for the school.
2. Separate user groups using VLANs.
3. Use the assigned `172.30.4.0/23` IPv4 block.
4. Support IPv4 and IPv6 dual-stack networking.
5. Provide a dedicated management network.
6. Provide Guest Wi-Fi.
7. Isolate Guest users from internal school resources.
8. Provide a scalable design suitable for Cisco Packet Tracer implementation and testing.
