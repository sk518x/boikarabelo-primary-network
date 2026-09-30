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

The Guest Wi-Fi network was tested using a wireless test laptop connected to the `GUEST` SSID.

The test laptop was configured with:

* IPv4 address: `172.30.4.131`
* Subnet mask: `255.255.255.192`
* Default gateway: `172.30.4.129`

First, the Guest device successfully reached its own default gateway:

```text
ping 172.30.4.129
```

This confirmed that the Guest device could communicate with the Guest VLAN gateway.

The Guest device was then tested against internal networks. The following traffic was blocked:

| Test               | Result  |
| ------------------ | ------- |
| Guest → Admin      | Blocked |
| Guest → Staff      | Blocked |
| Guest → Lab        | Blocked |
| Guest → Library    | Blocked |
| Guest → Management | Blocked |

The Guest isolation is enforced using the `GUEST_ISOLATION` extended IPv4 ACL on R1.

The ACL was also checked using:

```text
show access-lists GUEST_ISOLATION
```

The ACL counters confirmed that the configured deny rules were matching Guest traffic.

Evidence:

* `evidence/security/guest-to-gateway.png`
* `evidence/security/guest-isolation-tests-1.1.png`
* `evidence/security/guest-isolation-tests-1.2.png`
* `evidence/security/guest-isolation-acl-results.png`
* `evidence/security/guest-isolation-acl.png`

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
