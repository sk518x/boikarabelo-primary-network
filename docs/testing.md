# Testing and Verification

## 1. Purpose

Testing was performed to verify that the implemented network meets the client requirements and that the configured VLANs, IPv4 addressing, IPv6 addressing, routing, Guest Wi-Fi isolation and SSH remote management operate as intended.

## 2. VLAN Verification

The VLAN configuration was verified on the switches using the `show vlan brief` command.

The required VLANs were configured:

* VLAN 10 – Admin
* VLAN 20 – Staff
* VLAN 30 – Lab
* VLAN 40 – Library
* VLAN 50 – Guest
* VLAN 99 – Management

The VLAN configuration was successfully displayed on SW1.

Evidence:

`evidence/vlan/vlan-configuration.png`

---

## 3. IPv4 Connectivity Testing

IPv4 connectivity between the internal networks was tested using ICMP ping.

The following tests were successful:

* Admin → Staff
* Admin → Lab
* Admin → Library

These tests confirmed that the configured IPv4 addressing, VLAN segmentation and inter-VLAN routing were functioning.

Evidence:

* `evidence/ipv4/admin-to-staff-ipv4.png`
* `evidence/ipv4/admin-to-lab-ipv4.png`
* `evidence/ipv4/admin-to-library-ipv4.png`

---

## 4. IPv6 Connectivity Testing

IPv6 connectivity was tested between different VLANs.

The following tests were successful:

* Admin → Staff
* Lab → Admin

These tests confirmed that the configured IPv6 addresses and router-on-a-stick interfaces allowed communication between the IPv6 VLAN networks.

Evidence:

* `evidence/ipv6/admin-to-staff-ipv6.png`
* `evidence/ipv6/lab-to-admin-ipv6.png`

---

## 5. IPv6 Routing Verification

The IPv6 routing table on R1 was checked using:

```text
show ipv6 route
```

The routing table showed connected routes for the six configured IPv6 VLAN networks.

This verified that R1 had the required IPv6 networks available through its VLAN subinterfaces.

Evidence:

`evidence/ipv6/ipv6-routing-table.png`

---
## 6. Guest Wi-Fi Isolation Testing

The Guest Wi-Fi network uses VLAN 50 and the IPv4 subnet `172.30.4.128/26`. Guest traffic is restricted from accessing the internal school networks. Guest IPv6 traffic is also restricted from accessing the internal IPv6 networks.

### 6.1 Guest IPv4 Isolation

A temporary wireless test laptop, GUEST-TEST, was used to verify the Guest network. The device was configured with:

* IPv4 address: `172.30.4.131`
* Subnet mask: `255.255.255.192`
* Default gateway: `172.30.4.129`

The Guest device was first tested against its own gateway. The test was successful, confirming that the Guest device could communicate with the Guest VLAN gateway.

The Guest device was then tested against the internal IPv4 networks:

* Admin: `172.30.4.194`
* Staff: `172.30.4.66`
* Lab: `172.30.4.2`
* Library: `172.30.4.226`
* Management: `172.30.5.2`

These connections were blocked by the `GUEST_ISOLATION` IPv4 access-control list.

The IPv4 Guest isolation ACL was verified on R1 using:

```text
show access-lists GUEST_ISOLATION
```

The ACL denies Guest traffic to the internal IPv4 networks and permits other IPv4 traffic.

### 6.2 Guest IPv6 Isolation

Guest IPv6 isolation was also implemented and tested to ensure that the Guest network could not bypass the security restrictions by using IPv6.

The Guest IPv6 access-control list, `GUEST_V6_ISOLATION`, was applied inbound on the R1 Guest subinterface:

```text
interface gigabitEthernet 0/0.50
ipv6 traffic-filter GUEST_V6_ISOLATION in
```
The IPv6 Guest isolation ACL was verified on R1 using:

show ipv6 access-list GUEST_V6_ISOLATION

The Guest IPv6 connection was tested against the Guest VLAN gateway. The gateway connection was successful.

The Guest device was then tested against the internal IPv6 networks:

* Admin: `2001:DB8:AC30:1::/64`
* Staff: `2001:DB8:AC30:2::/64`
* Lab: `2001:DB8:AC30:3::/64`
* Library: `2001:DB8:AC30:4::/64`
* Management: `2001:DB8:AC30:6::/64`

The tests to the internal IPv6 networks were blocked by the `GUEST_V6_ISOLATION` access-control list.

This confirms that Guest isolation is implemented for both IPv4 and IPv6 traffic.

### 6.3 Guest Isolation Result

The Guest Wi-Fi network was successfully tested for isolation from the internal school networks.

| Test                       | Result     |
| -------------------------- | ---------- |
| Guest → IPv4 Guest Gateway | Successful |
| Guest → Admin IPv4         | Blocked    |
| Guest → Staff IPv4         | Blocked    |
| Guest → Lab IPv4           | Blocked    |
| Guest → Library IPv4       | Blocked    |
| Guest → Management IPv4    | Blocked    |
| Guest → IPv6 Guest Gateway | Successful |
| Guest → Admin IPv6         | Blocked    |
| Guest → Staff IPv6         | Blocked    |
| Guest → Lab IPv6           | Blocked    |
| Guest → Library IPv6       | Blocked    |
| Guest → Management IPv6    | Blocked    |

The results demonstrate that the Guest network is separated from the internal school networks while maintaining connectivity to its own gateway.

### Guest Isolation Evidence

The following evidence supports the Guest Wi-Fi security implementation:

- `evidence/security/guest-isolation-acl.png` – IPv4 Guest isolation ACL
- `evidence/security/guest-to-gateway.png` – Successful Guest-to-gateway test
- `evidence/security/guest-isolation-tests.png` – IPv4 Guest isolation tests
- `evidence/security/guest-ipv6-isolation.png` – IPv6 Guest isolation test

---

## 7. SSH Remote Management Testing

SSH remote management was configured on R1 and the switches.

SSH connectivity was successfully tested from PC-ADMIN to the network devices.

The following devices were successfully accessed using SSH:

* R1
* SW2
* SW3
* SW4
* SW5

For example, R1 was accessed using:

```text
ssh -l admin 172.30.5.1
```

The successful login confirmed that SSH remote management was operational.

Evidence:

* `evidence/ssh/ssh-configuration.png`
* `evidence/ssh/ssh-r1-test.png`

---

## 8. Test Summary

| Area            | Test                            | Result     |
| --------------- | ------------------------------- | ---------- |
| VLANs           | VLAN configuration verification | Successful |
| IPv4            | Admin → Staff                   | Successful |
| IPv4            | Admin → Lab                     | Successful |
| IPv4            | Admin → Library                 | Successful |
| IPv6            | Admin → Staff                   | Successful |
| IPv6            | Lab → Admin                     | Successful |
| IPv6            | Routing table verification      | Successful |
| Guest Wi-Fi     | Guest → Gateway                 | Successful |
| Guest isolation | Guest → Admin                   | Blocked    |
| Guest isolation | Guest → Staff                   | Blocked    |
| Guest isolation | Guest → Lab                     | Blocked    |
| Guest isolation | Guest → Library                 | Blocked    |
| Guest isolation | Guest → Management              | Blocked    |
| SSH             | PC-ADMIN → R1/SW2–SW5           | Successful |

---

## 9. Conclusion

The implemented network was tested against the main functionality required for Milestone 2. Internal IPv4 and IPv6 connectivity was verified, the IPv6 routing table was checked, Guest Wi-Fi isolation was tested using a wireless test device, and SSH remote management was successfully demonstrated.

The test results provide evidence that the implemented configuration operates as designed for the tested network requirements.
