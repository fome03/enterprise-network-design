# 🌐 Enterprise Network Infrastructure Design

## 📌 Project Overview

This project demonstrates the design and documentation of a secure, scalable enterprise network infrastructure for a simulated organization.

The network supports multiple departments, corporate users, IT administrators, internal servers, wireless devices, guest users, and network management infrastructure while maintaining logical separation between different categories of network traffic.

The project applies practical networking and security concepts including VLAN segmentation, IPv4 subnetting, Layer 2 switching, Layer 3 routing, DHCP planning, DNS, wireless networking, access control, and network security.

---

## 🗺️ Network Topology

The following Microsoft Visio diagram illustrates the logical architecture of the simulated enterprise network, including perimeter security, Layer 3 routing, access switching, server infrastructure, wireless connectivity, and VLAN segmentation.

![Enterprise Network Topology](diagrams/network-topology.png)

---

## 📁 Project Documentation

Explore the technical documentation for this project:

- [IP Addressing & VLAN Plan](addressing/ip-addressing-plan.md) — VLAN assignments, subnet structure, default gateways, DHCP ranges, and addressing strategy.
- [Network Design Documentation](documentation/network-design.md) — Architecture, switching, routing, wireless connectivity, traffic flow, and scalability.
- [Security Considerations](documentation/security-considerations.md) — Network segmentation, ACL strategy, guest isolation, management security, firewalling, endpoint protection, and monitoring.
- [Example Cisco Configurations](configs/example-configurations.md) — Cisco IOS-style examples for VLANs, SVIs, trunking, DHCP, ACLs, port security, SSH, and verification commands.
- [Network Topology Diagram](diagrams/network-topology.png) — Microsoft Visio diagram illustrating the complete simulated enterprise architecture.

---

## 🎯 Project Objectives

- Design a scalable enterprise network architecture
- Segment network traffic using VLANs
- Develop an organized IPv4 addressing and subnetting scheme
- Separate corporate, IT, server, management, and guest traffic
- Design Layer 3 inter-VLAN routing
- Plan DHCP and DNS services
- Incorporate secure corporate and guest wireless access
- Apply network security and access-control principles
- Restrict access to network management infrastructure
- Document network architecture and technical design decisions

---

## 🏢 Network Scenario

The simulated organization operates within a multi-department office environment requiring reliable wired and wireless connectivity.

The network supports:

- Corporate employees
- IT and network administrators
- Internal servers
- Corporate wireless devices
- Guest wireless users
- Network infrastructure and management devices

Different categories of users, devices, and services are separated into dedicated VLANs to improve security, performance, scalability, and network management.

---

## 🌐 VLAN & IP Addressing Architecture

| VLAN | Network | Subnet | Default Gateway |
|---|---|---|---|
| 10 | Management | `10.10.10.0/24` | `10.10.10.1` |
| 20 | Corporate Users | `10.10.20.0/24` | `10.10.20.1` |
| 30 | IT Department | `10.10.30.0/24` | `10.10.30.1` |
| 40 | Servers | `10.10.40.0/24` | `10.10.40.1` |
| 50 | Corporate Wi-Fi | `10.10.50.0/24` | `10.10.50.1` |
| 60 | Guest Wi-Fi | `10.10.60.0/24` | `10.10.60.1` |

The addressing scheme uses the private `10.10.0.0/16` address space and assigns a dedicated `/24` subnet to each VLAN.

---

## 🛠️ Technologies & Concepts

### Networking

- TCP/IP
- IPv4
- Subnetting
- VLANs
- IEEE 802.1Q Trunking
- Layer 2 Switching
- Layer 3 Switching
- Inter-VLAN Routing
- DHCP
- DNS
- Ethernet
- Wireless Networking

### Infrastructure

- Layer 3 Core Switching
- Access Switching
- Wireless Access Points
- Perimeter Firewall
- Server Infrastructure
- Network Management Infrastructure
- MDF / IDF Design Concepts
- Cat6 and Fiber Infrastructure Planning

### Security

- Network Segmentation
- Access Control Lists (ACLs)
- Guest Network Isolation
- Dedicated Management VLAN
- Least-Privilege Network Access
- Firewall Policy Planning
- Secure Remote Administration
- Switch Port Security
- Network Monitoring and Logging

### Documentation & Tools

- Microsoft Visio
- Network Topology Documentation
- IP Addressing Plans
- VLAN Documentation
- Cisco IOS-Style Configuration Examples
- Infrastructure Documentation

---

## 🔐 Security Design

The network applies a defense-in-depth approach using multiple logical security controls.

Key design decisions include:

- Separating users, servers, administrators, guests, and infrastructure into dedicated VLANs
- Restricting Guest Wi-Fi from accessing internal enterprise networks
- Isolating network management interfaces within VLAN 10
- Using ACLs to control inter-VLAN communication
- Positioning a firewall between the internal network and the Internet
- Applying least-privilege principles to network access
- Using secure protocols such as SSH for administrative access
- Planning centralized logging and monitoring capabilities

More detailed security analysis is available in the [Security Considerations](documentation/security-considerations.md) documentation.

---

## 🔄 Example Traffic Flow

Internet-bound corporate traffic:

```text
Corporate Endpoint
        ↓
   Access Switch
        ↓
Core Layer 3 Switch
        ↓
     Firewall
        ↓
     Internet
```

Internal communication requiring routing:

```text
Corporate User VLAN 20
        ↓
Core Layer 3 Switch
        ↓
    Server VLAN 40
```

Guest Internet access:

```text
Guest Device
     ↓
Wireless Access Point
     ↓
Guest VLAN 60
     ↓
Core Layer 3 Switch
     ↓
   Firewall
     ↓
   Internet
```

Guest traffic is restricted from accessing internal enterprise VLANs.

---

## ⚙️ Example Configuration Coverage

Cisco IOS-style configuration examples are included to demonstrate how portions of the logical design could be implemented.

Examples include:

- VLAN creation
- Switched Virtual Interfaces (SVIs)
- Inter-VLAN routing
- Access ports
- 802.1Q trunks
- DHCP pools
- Guest-network ACLs
- Management access restrictions
- Switch port security
- SSH administration
- Network verification commands

See [Example Cisco Configurations](configs/example-configurations.md) for the complete examples.

---

## 📊 Implementation Status

This repository currently represents the **design and documentation phase** of the simulated enterprise network.

Completed:

- ✅ Logical network architecture
- ✅ Microsoft Visio topology
- ✅ VLAN segmentation strategy
- ✅ IPv4 addressing plan
- ✅ DHCP addressing plan
- ✅ Security architecture
- ✅ ACL strategy
- ✅ Cisco IOS-style configuration examples
- ✅ Technical documentation

Potential future implementation:

- ⏳ Cisco Packet Tracer deployment
- ⏳ VLAN and trunk configuration testing
- ⏳ Inter-VLAN routing validation
- ⏳ DHCP connectivity testing
- ⏳ ACL enforcement testing
- ⏳ End-to-end connectivity verification

---

## 🗂️ Repository Structure

```text
enterprise-network-design/
│
├── addressing/
│   └── ip-addressing-plan.md
│
├── configs/
│   └── example-configurations.md
│
├── diagrams/
│   └── network-topology.png
│
├── documentation/
│   ├── network-design.md
│   └── security-considerations.md
│
├── LICENSE
└── README.md
```

---

## 🚀 Future Improvements

Future versions of this project could incorporate:

- Cisco Packet Tracer implementation
- Redundant core switching
- Firewall high availability
- Spanning Tree Protocol optimization
- EtherChannel
- Network Access Control (NAC)
- 802.1X authentication
- IDS/IPS integration
- Centralized SIEM monitoring
- Automated configuration backups
- Network monitoring and alerting
- Additional routing and redundancy protocols

---

## 👩🏾‍💻 Author

**Enifome Ebe**  
Computer Information Systems — University of Houston  
Accelerated B.S./M.S. Cybersecurity Program  
Minor in Legal Studies

**Areas of Interest:** Cybersecurity • Network Engineering • IT Infrastructure • Digital Forensics
