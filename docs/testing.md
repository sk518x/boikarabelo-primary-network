# Milestone 2 Testing

## 1. Purpose

Testing was performed in Cisco Packet Tracer to verify that the implemented network provides the required connectivity, IPv6 functionality, Guest isolation and remote network management.

## 2. VLAN Verification

The VLAN configuration was checked using:

```text
show vlan brief
```

The required VLANs were verified:

* VLAN 10 – Admin
* VLAN 20 – Staff
* VLAN 30 – Lab
* VLAN 40 – Library
* VLAN 50 – Guest
* VLAN 99 – Management

Evidence:

`../evidence/vlan/vlan-configuration.png`

## 3. IPv4 Connectivity Tests

### Admin → Staff

**Source:** `PC-ADMIN`
**Destination:** `172.30.4.66`

**Result:** Successful — 4/4 packets received, 0% packet loss.

Evidence:

`../evidence/ipv4/admin-to-staff-ipv4.png`

### Admin → Lab

**Source:** `PC-ADMIN`
**Destination:** `172.30.4.2`

**Result:** Successful — 4/4 packets received, 0% packet loss.

Evidence:

`../evidence/ipv4/admin-to-lab-ipv4.png`

### Admin → Library

**Source:** `PC-ADMIN`
**Destination:** `172.30.4.226`

**Result:** Successful — 4/4 packets received, 0% packet loss.

Evidence:

`../evidence/ipv4/admin-to-library-ipv4.png`

These tests verify IPv4 connectivity between the Admin network and the Staff, Lab and Library VLANs through R1.

## 4. IPv6 Connectivity Tests

### Admin → Staff

**Source:** `PC-ADMIN`
**Destination:** `2001:DB8:AC30:2::10`

**Result:** Successful — 4/4 packets received, 0% packet loss.

Evidence:

`../evidence/ipv6/admin-to-staff-ipv6.png`

### Lab → Admin

**Source:** `PC-LAB`
**Destination:** `2001:DB8:AC30:1::10`

**Result:** Successful — 4/4 packets received, 0% packet loss.

Evidence:

`../evidence/ipv6/lab-to-admin-ipv6.png`

These tests verify IPv6 connectivity between separate VLANs.

## 5. IPv6 Routing Verification

R1's IPv6 routing table was checked using:

```text
show ipv6 route
```

The routing table contained connected routes for the configured VLAN IPv6 networks.

Evidence:

`../evidence/ipv6/ipv6-routing-table.png`

This confirms that R1 has IPv6 routes for the configured VLAN networks.

## 6. Guest Isolation Verification

The Guest isolation ACL was checked using:

```text
show access-lists GUEST_ISOLATION
```

The ACL contains rules denying Guest traffic to the internal Admin, Staff, Lab, Library and Management networks.

Evidence:

`../evidence/security/guest-isolation-acl.png`

The ACL is applied inbound on R1's Guest subinterface:

```text
Gi0/0.50
```

This verifies the configuration used to implement the Guest Wi-Fi isolation requirement.

## 7. SSH Verification

SSH was configured for secure remote management of the network devices.

The SSH configuration includes:

```text
line vty 0 4
 login local
 transport input ssh
```

Evidence:

`../evidence/ssh/ssh-configuration.png`

A successful SSH connection from PC-ADMIN to R1 was also tested.

Evidence:

`../evidence/ssh/ssh-r1-test.png`

The successful connection confirms that SSH remote management is operational.

## 8. Test Summary

| Test                 | Result     |
| -------------------- | ---------- |
| VLAN configuration   | Verified   |
| Admin → Staff IPv4   | Successful |
| Admin → Lab IPv4     | Successful |
| Admin → Library IPv4 | Successful |
| Admin → Staff IPv6   | Successful |
| Lab → Admin IPv6     | Successful |
| IPv6 routing table   | Verified   |
| Guest isolation ACL  | Verified   |
| SSH configuration    | Verified   |
| SSH connection to R1 | Successful |

## 9. Conclusion

The testing confirms the main implemented networking functions required for Milestone 2, including VLAN segmentation, IPv4 connectivity, IPv6 connectivity and routing, Guest network isolation configuration and SSH-based remote management.
