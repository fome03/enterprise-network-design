# Security Considerations

## 1. Security Overview

The enterprise network is designed using a defense-in-depth approach in which multiple security controls work together to reduce risk.

Rather than placing all devices on a single network, the architecture separates users, administrators, servers, wireless devices, guests, and network infrastructure into dedicated VLANs.

This segmentation limits unnecessary communication between systems and provides logical boundaries where security policies can be applied.

---

## 2. Network Segmentation

VLAN segmentation is one of the primary security controls used in this design.

The network separates traffic into the following security zones:

- VLAN 10 – Management
- VLAN 20 – Corporate Users
- VLAN 30 – IT Department
- VLAN 40 – Servers
- VLAN 50 – Corporate Wi-Fi
- VLAN 60 – Guest Wi-Fi

Separating these environments reduces the potential impact of a compromised endpoint and allows communication between network segments to be controlled.

For example, a device connected to the Guest Wi-Fi network should not be able to directly communicate with internal servers or network management interfaces.

---

## 3. Management VLAN Security

VLAN 10 is dedicated to network infrastructure management.

This VLAN may contain management interfaces for:

- Switches
- Firewalls
- Wireless access points
- Other managed network infrastructure

Access to the Management VLAN should be restricted to authorized IT administrators and approved administrative devices.

Corporate users and guest devices should not have direct access to network management interfaces.

Additional protections may include:

- Multi-factor authentication
- Strong administrative passwords
- Role-based access control
- Secure management protocols such as SSH and HTTPS
- Administrative logging and monitoring

Unencrypted management protocols such as Telnet should be disabled.

---

## 4. Access Control Lists

Access Control Lists (ACLs) can be configured on the Layer 3 switch to control communication between VLANs.

Example policies include:

### Guest Network

VLAN 60 should:

- Be allowed to access the Internet
- Be denied access to VLAN 10
- Be denied access to VLAN 20
- Be denied access to VLAN 30
- Be denied access to VLAN 40
- Be denied access to VLAN 50

### Corporate Users

VLAN 20 may:

- Access approved internal services
- Access the Internet
- Be restricted from directly accessing network management interfaces

### IT Department

VLAN 30 may receive additional access to systems required for administration and troubleshooting.

Access should still follow the principle of least privilege.

### Server Network

VLAN 40 should only accept traffic required for approved business services.

Unnecessary communication from user networks should be blocked.

---

## 5. Perimeter Firewall

The perimeter firewall provides a security boundary between the internal enterprise network and the Internet.

Potential firewall responsibilities include:

- Stateful traffic inspection
- Network Address Translation (NAT)
- Inbound traffic filtering
- Outbound traffic filtering
- Security policy enforcement
- Logging suspicious traffic
- Blocking unauthorized connections

Inbound Internet traffic should be denied by default unless a specific business requirement requires an externally accessible service.

---

## 6. Guest Wi-Fi Isolation

Guest wireless devices are assigned to VLAN 60.

Guest Wi-Fi should provide Internet connectivity without providing access to internal enterprise resources.

Guest devices should be isolated from:

- Corporate workstations
- IT systems
- Internal servers
- Network management interfaces
- Corporate wireless devices

Client isolation can also be implemented to prevent guest devices from communicating directly with other guest devices.

---

## 7. Corporate Wireless Security

Corporate Wi-Fi uses VLAN 50 and is intended for authorized devices.

Potential wireless security controls include:

- WPA2-Enterprise or WPA3-Enterprise
- 802.1X authentication
- Centralized identity-based authentication
- Strong encryption
- Authorized device policies

Corporate and guest wireless traffic remain logically separated even when wireless access points support both networks.

---

## 8. Server Security

Servers are placed in VLAN 40 rather than directly inside user networks.

Security controls for servers may include:

- Host-based firewalls
- Endpoint protection
- Regular security updates
- Vulnerability management
- Access logging
- Role-based access control
- Restricted administrative access
- Regular backups

Only required ports and services should be accessible from other VLANs.

---

## 9. Endpoint Security

Corporate and IT endpoints should follow organizational security policies.

Potential controls include:

- Endpoint detection and response (EDR)
- Antivirus protection
- Operating system patching
- Disk encryption
- Screen-lock policies
- Multi-factor authentication
- Least-privilege user accounts
- Application control

Compromised endpoints should be isolated from the network when suspicious activity is detected.

---

## 10. Monitoring and Logging

Network and security devices should generate logs that can be reviewed for suspicious activity.

Useful log sources include:

- Firewall logs
- Switch logs
- Authentication logs
- Server logs
- Wireless infrastructure logs
- Endpoint security logs

In a larger enterprise environment, logs could be forwarded to a centralized Security Information and Event Management (SIEM) platform.

Centralized monitoring can help identify:

- Repeated authentication failures
- Unauthorized access attempts
- Unusual network traffic
- Communication with suspicious destinations
- Changes to network infrastructure

---

## 11. Principle of Least Privilege

Access between network segments should follow the principle of least privilege.

Devices and users should only receive the network access necessary to perform their required functions.

For example:

`Guest User → Internet: ALLOW`

`Guest User → Internal Server: DENY`

`Corporate User → Approved Application Server: ALLOW`

`Corporate User → Network Management Interface: DENY`

`Authorized IT Administrator → Network Management Interface: ALLOW`

---

## 12. Future Security Improvements

The network could be strengthened further through technologies such as:

- Intrusion Detection and Prevention Systems (IDS/IPS)
- Network Access Control (NAC)
- Centralized SIEM monitoring
- 802.1X wired authentication
- Redundant firewalls
- Redundant core switches
- Automated configuration backups
- Vulnerability scanning
- Network behavior monitoring
- Zero Trust access principles

---

## Security Summary

The security architecture uses segmentation, restricted management access, firewall protection, ACLs, wireless isolation, endpoint security, and monitoring to reduce the attack surface of the simulated enterprise network.

The overall objective is to ensure that network access is granted according to business requirements while preventing unauthorized communication between network segments.
