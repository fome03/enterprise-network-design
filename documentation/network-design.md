# Enterprise Network Design Documentation

## 1. Design Overview

This project represents a simulated enterprise network designed to provide secure, scalable, and organized connectivity for corporate users, IT administrators, internal servers, wireless devices, and guests.

The architecture uses VLAN segmentation to separate different categories of network traffic. A Layer 3 core switch provides inter-VLAN routing, while access switches connect end-user devices and a dedicated server switch supports internal services.

The design also incorporates a perimeter firewall, dedicated management network, corporate wireless network, and isolated guest wireless network.

---

## 2. Network Architecture

The network follows a hierarchical structure consisting of perimeter security, core networking, access switching, server infrastructure, and endpoint connectivity.

### Internet and Perimeter Firewall

External connectivity enters the network through a perimeter firewall.

The firewall serves as the security boundary between the internal enterprise network and the Internet. It can provide services such as:

- Traffic filtering
- Stateful inspection
- Network Address Translation (NAT)
- Inbound and outbound access control
- Security policy enforcement

The firewall connects directly to the Core Layer 3 Switch.

### Core Layer 3 Switch

The Core Layer 3 Switch serves as the central point of the internal network.

Its primary responsibilities include:

- Inter-VLAN routing
- Default gateway services for internal VLANs
- Connectivity between access switches and server infrastructure
- Connectivity to wireless infrastructure
- Forwarding traffic toward the perimeter firewall

Each VLAN uses a gateway address ending in `.1`, representing a switched virtual interface (SVI) configured on the Layer 3 switch.

For example:

- VLAN 20 → `10.10.20.1`
- VLAN 30 → `10.10.30.1`
- VLAN 40 → `10.10.40.1`

---

## 3. Access Layer

### Access Switch 1

Access Switch 1 primarily supports corporate users.

Connected devices may include:

- Employee workstations
- VoIP phones
- Other authorized wired endpoints

These devices are assigned to VLAN 20.

### Access Switch 2

Access Switch 2 supports IT department users and technical workstations.

IT endpoints are assigned to VLAN 30, separating administrative and technical users from general corporate devices.

### Server Switch

The server switch provides connectivity for internal enterprise services.

Example systems include:

- File Server
- Application Server
- Database Server

Servers are assigned to VLAN 40.

Separating servers from user networks allows access to internal services to be controlled through routing and security policies.

---

## 4. VLAN Architecture

The network uses six VLANs to logically separate traffic.

| VLAN | Name | Subnet | Default Gateway |
|---|---|---|---|
| 10 | Management | 10.10.10.0/24 | 10.10.10.1 |
| 20 | Corporate Users | 10.10.20.0/24 | 10.10.20.1 |
| 30 | IT Department | 10.10.30.0/24 | 10.10.30.1 |
| 40 | Servers | 10.10.40.0/24 | 10.10.40.1 |
| 50 | Corporate Wi-Fi | 10.10.50.0/24 | 10.10.50.1 |
| 60 | Guest Wi-Fi | 10.10.60.0/24 | 10.10.60.1 |

This segmentation reduces unnecessary broadcast traffic and provides logical boundaries that can be used to enforce security policies.

---

## 5. Management Network

VLAN 10 is reserved for network management.

The management network can be used for administrative access to infrastructure such as:

- Switches
- Firewalls
- Wireless access points
- Other managed network devices

Access to the management VLAN should be restricted to authorized administrative systems and personnel.

General corporate and guest devices should not have direct access to this network.

---

## 6. Wireless Architecture

The wireless infrastructure supports two logically separated wireless networks.

### Corporate Wi-Fi

Corporate wireless devices are assigned to VLAN 50.

This network is intended for authorized employee devices requiring access to approved enterprise resources.

### Guest Wi-Fi

Guest wireless devices are assigned to VLAN 60.

The guest network is designed to provide Internet connectivity while remaining isolated from internal corporate networks, servers, and management infrastructure.

This separation reduces the risk of unmanaged guest devices accessing enterprise resources.

---

## 7. Inter-VLAN Routing

Devices within the same VLAN communicate through Layer 2 switching.

Communication between different VLANs requires Layer 3 routing.

The Core Layer 3 Switch performs inter-VLAN routing using switched virtual interfaces associated with each VLAN.

Access Control Lists (ACLs) can be applied to restrict communication between VLANs according to organizational security requirements.

For example:

- Corporate users may access approved application servers.
- Guest devices should not access internal VLANs.
- Management resources should only be accessible from authorized administrative systems.
- Server access can be limited based on business requirements.

---

## 8. Network Traffic Flow

A typical Internet-bound connection follows this path:

`Endpoint → Access Switch → Core Layer 3 Switch → Firewall → Internet`

Communication between internal VLANs follows:

`Source Device → Access Switch → Core Layer 3 Switch → Destination Network`

Wireless traffic follows:

`Wireless Device → Wireless Access Point → Core Layer 3 Switch → Destination`

Guest wireless traffic intended for the Internet follows:

`Guest Device → Wireless Access Point → Core Layer 3 Switch → Firewall → Internet`

---

## 9. Scalability

The design allows the simulated organization to expand without redesigning the entire network.

Additional:

- Access switches
- Wireless access points
- End-user devices
- Servers
- VLANs
- Subnets

can be introduced as organizational requirements grow.

The structured VLAN and addressing scheme also makes new network segments easier to identify, document, and manage.

---

## 10. Design Summary

This network design demonstrates several core enterprise networking concepts:

- Hierarchical network architecture
- VLAN segmentation
- IPv4 subnetting
- Layer 2 switching
- Layer 3 routing
- Inter-VLAN communication
- Wireless network segmentation
- Server network separation
- Guest network isolation
- Network management separation
- Perimeter firewall placement
- Access control planning

The architecture provides a foundation that can be expanded with additional security controls, redundancy, monitoring, and network automation.
