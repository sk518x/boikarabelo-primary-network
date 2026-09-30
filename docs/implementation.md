# Milestone 2 Implementation

## Overview

This document records the implementation of the Boikarabelo Primary School network in Cisco Packet Tracer.

The implementation follows the network design developed for CMPG 325 Project ID CMPG325-2026-024.

## Network Devices

The implemented topology contains:

* R1 – Router and inter-VLAN routing
* SW1 – Core switch
* SW2 – Admin access switch
* SW3 – Staff access switch
* SW4 – Lab access switch
* SW5 – Library and Guest access switch
* AP1 – Guest wireless access point
* Representative wired and wireless end devices

SW5 provides wired connectivity for the Library network and a separate connection to AP1 for the Guest Wi-Fi network.

## VLAN Configuration

The following VLANs were implemented:

| VLAN | Name       | Purpose                     |
| ---- | ---------- | --------------------------- |
| 10   | Admin      | Administration              |
| 20   | Staff      | Teachers and staff          |
| 30   | Lab        | Student computer laboratory |
| 40   | Library    | Library/media computers     |
| 50   | Guest      | Visitor Wi-Fi               |
| 99   | Management | Network-device management   |

## IPv4 Implementation

The assigned `172.30.4.0/23` address block was subnetted using VLSM.

The VLAN gateway addresses configured on R1 are:

* VLAN 10: `172.30.4.193/27`
* VLAN 20: `172.30.4.65/26`
* VLAN 30: `172.30.4.1/26`
* VLAN 40: `172.30.4.225/27`
* VLAN 50: `172.30.4.129/26`
* VLAN 99: `172.30.5.1/28`

## IPv6 Implementation

The assigned IPv6 block is:

`2001:DB8:AC30::/48`

The implementation uses the selected `/60` design sub-block:

`2001:DB8:AC30:0::/60`

Each VLAN was assigned a `/64` IPv6 network.

R1 was configured with IPv6 gateway addresses for each VLAN, and IPv6 forwarding was enabled.

The IPv6 gateway addresses are:

* VLAN 10: `2001:DB8:AC30:1::1/64`
* VLAN 20: `2001:DB8:AC30:2::1/64`
* VLAN 30: `2001:DB8:AC30:3::1/64`
* VLAN 40: `2001:DB8:AC30:4::1/64`
* VLAN 50: `2001:DB8:AC30:5::1/64`
* VLAN 99: `2001:DB8:AC30:6::1/64`

## Inter-VLAN Routing

R1 uses a router-on-a-stick configuration with 802.1Q subinterfaces.

The VLAN subinterfaces are:

* `Gi0/0.10`
* `Gi0/0.20`
* `Gi0/0.30`
* `Gi0/0.40`
* `Gi0/0.50`
* `Gi0/0.99`

These subinterfaces provide the default gateways and enable communication between the VLANs.

## Guest Wi-Fi

VLAN 50 provides the Guest Wi-Fi network.

The wireless access point uses the `GUEST` SSID.

An extended ACL named `GUEST_ISOLATION` is applied inbound to `Gi0/0.50`.

The ACL blocks traffic from the Guest network to the internal Admin, Staff, Lab, Library, and Management networks while permitting other IP traffic.

This implements the client change request requiring Guest Wi-Fi to be isolated from internal school resources.

### Guest Wi-Fi Security

VLAN 50 is reserved for Guest Wi-Fi. Guest traffic is isolated from the internal school networks using access-control rules on R1.

For IPv4 traffic, the `GUEST_ISOLATION` extended access-control list prevents Guest clients from reaching the Admin, Staff, Lab, Library and Management IPv4 networks.

For IPv6 traffic, the `GUEST_V6_ISOLATION` IPv6 access-control list prevents Guest clients from reaching the internal IPv6 networks. The IPv6 access-control list is applied inbound on the Guest subinterface:

```text
interface gigabitEthernet 0/0.50
ipv6 traffic-filter GUEST_V6_ISOLATION in
```

Guest isolation was tested using both IPv4 and IPv6 connectivity tests. Guest clients could reach their respective Guest gateway but were prevented from accessing the internal school networks.


## Remote Management

VLAN 99 is used for network-device management.

SSH was configured on R1 and the access switches.

SSH connectivity was tested from PC-ADMIN to R1 and the access switches, confirming that the network devices can be remotely managed using SSH.

## Verification

The implementation was verified using Cisco IOS commands including:

```text
show vlan brief
show ip interface brief
show ipv6 interface brief
show ipv6 route
show ipv6 access-list GUEST_V6_ISOLATION
show access-lists GUEST_ISOLATION
```

Connectivity was also tested using IPv4 and IPv6 ping tests between representative devices on different VLANs.

IPv4 connectivity was verified between:

* Admin and Staff
* Admin and Lab
* Admin and Library

IPv6 connectivity was verified between:

* Admin and Staff
* Lab and Admin

The IPv6 routing table was also checked to confirm that the six configured IPv6 VLAN networks were directly connected to R1.

Guest isolation was verified through the configured `GUEST_ISOLATION` ACL, which denies Guest traffic to the internal school networks.

SSH access was verified from PC-ADMIN to R1 and SW2–SW5.

