# Network Design

## 1. Physical Topology

The proposed physical topology contains one edge router, one core switch, four access switches, one wireless access point, and representative end-user devices.

### Devices

| Device | Quantity | Purpose |
|--------|----------|---------|
| Edge Router (R1) | 1 | External connectivity and inter-VLAN routing |
| Core Switch (SW1) | 1 | Central network connection point |
| Access Switches (SW2–SW5) | 4 | Provide wired access for Admin, Staff, Lab and Library |
| Wireless Access Point (AP1) | 1 | Provides Guest Wi-Fi |
| End-user devices | Multiple | Represent school users and Guest clients |

### Physical Structure

```text
                         INTERNET / ISP
                              |
                             R1
                              |
                         Core SW1
                    ______/ | | \______
                   /       /  |  \       \
                 SW2     SW3 SW4 SW5
                Admin   Staff Lab Library
                                      |
                                     AP1
                                      |
                                    Guest
```

The four access switches provide separate wired access areas. The Guest wireless network is provided through AP1.

---

## 2. Logical Topology

The network is logically segmented using VLANs.

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | Admin | Administration users |
| 20 | Staff | Teachers and staff |
| 30 | Lab | Student computer laboratory |
| 40 | Library | Library/media computers |
| 50 | Guest | Visitor Wi-Fi |
| 99 | Management | Network-device management |

R1 performs inter-VLAN routing for the proposed IPv4 and IPv6 dual-stack design.

---

## 3. VLAN Design

### VLAN 10 – Admin
Administration users and devices.

### VLAN 20 – Staff
Teachers and staff.

### VLAN 30 – Lab
Student laboratory computers.

### VLAN 40 – Library
Library/media computers.

### VLAN 50 – Guest
Visitor wireless access. Guest traffic is isolated from internal resources.

### VLAN 99 – Management
Network-device management and SSH administration.

---

## 4. Inter-VLAN Routing

R1 will provide inter-VLAN routing for the VLANs.

Trunk links will carry the required VLAN traffic between the router/core/access portions of the design. End-user devices will connect through access ports assigned to their appropriate VLAN.

The final Packet Tracer implementation will verify IPv4 and IPv6 routing.

---

## 5. Guest Network Isolation

VLAN 50 is dedicated to Guest Wi-Fi.

The final implementation will use access-control rules to prevent Guest users from accessing internal school resources while allowing the required external connectivity.

---

## 6. Remote Management

VLAN 99 is reserved for management traffic.

Network devices will be assigned management addresses in this network, and secure SSH remote management will be configured during implementation.

---

## 7. IPv6 Dual-Stack Design

The project brief requires IPv6 dual-stack addressing and routing.

The proposed documentation parent prefix is:

`2001:db8:ac30:0::/60`

The proposed `/64` allocations are:

| VLAN | Name | IPv6 Network | Gateway |
|------|------|--------------|---------|
| 10 | Admin | `2001:db8:ac30:1::/64` | `2001:db8:ac30:1::1` |
| 20 | Staff | `2001:db8:ac30:2::/64` | `2001:db8:ac30:2::1` |
| 30 | Lab | `2001:db8:ac30:3::/64` | `2001:db8:ac30:3::1` |
| 40 | Library | `2001:db8:ac30:4::/64` | `2001:db8:ac30:4::1` |
| 50 | Guest | `2001:db8:ac30:5::/64` | `2001:db8:ac30:5::1` |
| 99 | Management | `2001:db8:ac30:6::/64` | `2001:db8:ac30:6::1` |

This is a proposed design addressing scheme and is not presented as an IPv6 block explicitly supplied by the client brief.
