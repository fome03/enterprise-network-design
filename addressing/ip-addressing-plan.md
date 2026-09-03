# IP Addressing & VLAN Plan

## Overview

This addressing plan uses the private 10.10.0.0/16 address space to organize the simulated enterprise network.

Separate VLANs are used for different user, device, and infrastructure groups. This improves network organization, reduces broadcast domains, and allows security policies to control communication between different parts of the network.

## VLAN and Subnet Allocation

| VLAN ID | VLAN Name | Purpose | Subnet | Default Gateway |
|---|---|---|---|---|
| 10 | Management | Network infrastructure and administrative interfaces | 10.10.10.0/24 | 10.10.10.1 |
| 20 | Corporate-Users | Employee workstations and wired devices | 10.10.20.0/24 | 10.10.20.1 |
| 30 | IT | IT administrators and technical staff | 10.10.30.0/24 | 10.10.30.1 |
| 40 | Servers | Internal application and infrastructure servers | 10.10.40.0/24 | 10.10.40.1 |
| 50 | Corporate-WiFi | Authorized corporate wireless devices | 10.10.50.0/24 | 10.10.50.1 |
| 60 | Guest-WiFi | Guest and unmanaged wireless devices | 10.10.60.0/24 | 10.10.60.1 |

## Addressing Strategy

Each VLAN is assigned a /24 subnet.

A /24 network provides 256 total IPv4 addresses, with 254 usable host addresses after reserving the network and broadcast addresses.

The `.1` address of each subnet is reserved as the default gateway.

Example:

**Corporate Users — VLAN 20**

- Network Address: `10.10.20.0`
- Subnet Mask: `255.255.255.0`
- CIDR Prefix: `/24`
- Default Gateway: `10.10.20.1`
- Usable Host Range: `10.10.20.1 - 10.10.20.254`
- Broadcast Address: `10.10.20.255`

## DHCP Strategy

Dynamic addressing can be used for employee and wireless client devices.

Example allocation:

| VLAN | DHCP Range |
|---|---|
| Corporate Users | 10.10.20.50 - 10.10.20.200 |
| Corporate Wi-Fi | 10.10.50.50 - 10.10.50.220 |
| Guest Wi-Fi | 10.10.60.50 - 10.10.60.240 |

Addresses outside the DHCP pools can be reserved for infrastructure, printers, access points, or other systems requiring predictable addresses.

## Static Addressing

Static or reserved IP addresses should be used for critical infrastructure including:

- Routers
- Layer 3 switches
- Firewalls
- Servers
- Wireless access points
- Network management systems

This allows administrators to reliably locate and manage infrastructure devices.

## Network Segmentation

Devices within the same VLAN communicate at Layer 2.

Communication between different VLANs requires Layer 3 routing through a router, Layer 3 switch, or firewall.

This provides an opportunity to apply security policies between network segments.

For example:

- Guest Wi-Fi should not access internal corporate networks.
- Corporate users should have limited access to the Management VLAN.
- IT administrators may require management access to network infrastructure.
- Server access should be restricted according to business requirements.

## Design Considerations

The addressing scheme was designed to be:

- Easy to understand and troubleshoot
- Scalable for additional devices
- Organized by network function
- Compatible with VLAN-based segmentation
- Suitable for applying firewall and access-control policies
