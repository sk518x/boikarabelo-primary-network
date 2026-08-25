# IP Addressing Plan

## 1. IPv4 Addressing

The assigned IPv4 address block is:

`172.30.4.0/23`

VLSM is used to allocate separate networks for the different VLANs.

## 2. IPv4 Subnet Allocation

| VLAN | Name | Network Address | Prefix | Subnet Mask | First Usable | Last Usable | Broadcast | Gateway |
|------|------|-----------------|--------|-------------|--------------|-------------|-----------|---------|
| 30 | Lab | 172.30.4.0 | /26 | 255.255.255.192 | 172.30.4.1 | 172.30.4.62 | 172.30.4.63 | 172.30.4.1 |
| 20 | Staff | 172.30.4.64 | /26 | 255.255.255.192 | 172.30.4.65 | 172.30.4.126 | 172.30.4.127 | 172.30.4.65 |
| 50 | Guest | 172.30.4.128 | /26 | 255.255.255.192 | 172.30.4.129 | 172.30.4.190 | 172.30.4.191 | 172.30.4.129 |
| 10 | Admin | 172.30.4.192 | /27 | 255.255.255.224 | 172.30.4.193 | 172.30.4.222 | 172.30.4.223 | 172.30.4.193 |
| 40 | Library | 172.30.4.224 | /27 | 255.255.255.224 | 172.30.4.225 | 172.30.4.254 | 172.30.4.255 | 172.30.4.225 |
| 99 | Management | 172.30.5.0 | /28 | 255.255.255.240 | 172.30.5.1 | 172.30.5.14 | 172.30.5.15 | 172.30.5.1 |

## 3. WAN Addressing

A `/30` network is reserved for the point-to-point WAN link:

`172.30.5.16/30`

| Address Type | Address |
|--------------|---------|
| Network | 172.30.5.16 |
| R1 | 172.30.5.17 |
| ISP | 172.30.5.18 |
| Broadcast | 172.30.5.19 |

## 4. VLSM Justification

- `/26` provides 64 total addresses and 62 usable addresses. Used for Lab, Staff and Guest.
- `/27` provides 32 total addresses and 30 usable addresses. Used for Admin and Library.
- `/28` provides 16 total addresses and 14 usable addresses. Used for Management.
- `/30` provides 4 total addresses and 2 usable addresses. Used for the point-to-point WAN link.

## 5. IPv6 Addressing

IPv6 dual-stack addressing and routing is the assigned networking challenge.

The proposed documentation parent prefix is:

`2001:db8:ac30:0::/60`

Each VLAN receives a `/64` network:

| VLAN | Name | IPv6 Network | Gateway |
|------|------|--------------|---------|
| 10 | Admin | `2001:db8:ac30:1::/64` | `2001:db8:ac30:1::1` |
| 20 | Staff | `2001:db8:ac30:2::/64` | `2001:db8:ac30:2::1` |
| 30 | Lab | `2001:db8:ac30:3::/64` | `2001:db8:ac30:3::1` |
| 40 | Library | `2001:db8:ac30:4::/64` | `2001:db8:ac30:4::1` |
| 50 | Guest | `2001:db8:ac30:5::/64` | `2001:db8:ac30:5::1` |
| 99 | Management | `2001:db8:ac30:6::/64` | `2001:db8:ac30:6::1` |

**Note:** The project brief shown specifies the IPv4 block but does not explicitly provide an IPv6 block. The IPv6 prefix above is therefore a proposed documentation/design prefix for Milestone 1.

## 6. Dual-Stack Operation

The final network will operate using both IPv4 and IPv6. The router will provide routing for both protocols, and each VLAN will have corresponding IPv4 and IPv6 addressing.
