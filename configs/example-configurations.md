# Example Network Configurations

## Overview

The following configurations demonstrate how portions of the simulated enterprise network could be implemented using Cisco IOS-style commands.

These configurations are provided for educational and documentation purposes and are not taken from a production environment.

---

# 1. VLAN Configuration

The network uses six VLANs to separate users, infrastructure, servers, and wireless devices.

```cisco
configure terminal

vlan 10
 name MANAGEMENT

vlan 20
 name CORPORATE_USERS

vlan 30
 name IT_DEPARTMENT

vlan 40
 name SERVERS

vlan 50
 name CORPORATE_WIFI

vlan 60
 name GUEST_WIFI

end
```

The VLAN configuration creates logical Layer 2 network segments corresponding to the enterprise network design.

---

# 2. Layer 3 Switch – SVI Configuration

Switched Virtual Interfaces (SVIs) provide the default gateways for each VLAN.

```cisco
configure terminal

ip routing

interface vlan 10
 description Management Gateway
 ip address 10.10.10.1 255.255.255.0
 no shutdown

interface vlan 20
 description Corporate Users Gateway
 ip address 10.10.20.1 255.255.255.0
 no shutdown

interface vlan 30
 description IT Department Gateway
 ip address 10.10.30.1 255.255.255.0
 no shutdown

interface vlan 40
 description Server Network Gateway
 ip address 10.10.40.1 255.255.255.0
 no shutdown

interface vlan 50
 description Corporate WiFi Gateway
 ip address 10.10.50.1 255.255.255.0
 no shutdown

interface vlan 60
 description Guest WiFi Gateway
 ip address 10.10.60.1 255.255.255.0
 no shutdown

end
```

The `ip routing` command enables Layer 3 routing between the VLAN interfaces.

---

# 3. Access Port Configuration

An employee workstation connected to an access switch could be assigned to VLAN 20.

```cisco
configure terminal

interface GigabitEthernet1/0/1
 description Corporate Workstation
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown

end
```

An IT workstation could instead be assigned to VLAN 30.

```cisco
configure terminal

interface GigabitEthernet1/0/2
 description IT Workstation
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown

end
```

---

# 4. Trunk Configuration

Connections between access switches and the Core Layer 3 Switch can use IEEE 802.1Q trunking.

Example uplink:

```cisco
configure terminal

interface GigabitEthernet1/0/24
 description Uplink to Core Layer 3 Switch
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50,60
 no shutdown

end
```

A trunk allows traffic from multiple VLANs to traverse a single physical connection while maintaining logical separation.

---

# 5. DHCP Configuration

The Layer 3 switch or a dedicated DHCP server could provide dynamic addressing.

Example DHCP configuration for the Corporate Users VLAN:

```cisco
configure terminal

ip dhcp excluded-address 10.10.20.1 10.10.20.49
ip dhcp excluded-address 10.10.20.201 10.10.20.254

ip dhcp pool CORPORATE_USERS
 network 10.10.20.0 255.255.255.0
 default-router 10.10.20.1
 dns-server 10.10.40.10

end
```

This configuration provides addresses within the planned DHCP range:

`10.10.20.50 – 10.10.20.200`

---

# 6. Corporate Wi-Fi DHCP Pool

```cisco
configure terminal

ip dhcp excluded-address 10.10.50.1 10.10.50.49
ip dhcp excluded-address 10.10.50.221 10.10.50.254

ip dhcp pool CORPORATE_WIFI
 network 10.10.50.0 255.255.255.0
 default-router 10.10.50.1
 dns-server 10.10.40.10

end
```

Planned DHCP range:

`10.10.50.50 – 10.10.50.220`

---

# 7. Guest Wi-Fi DHCP Pool

```cisco
configure terminal

ip dhcp excluded-address 10.10.60.1 10.10.60.49
ip dhcp excluded-address 10.10.60.241 10.10.60.254

ip dhcp pool GUEST_WIFI
 network 10.10.60.0 255.255.255.0
 default-router 10.10.60.1
 dns-server 8.8.8.8

end
```

Planned DHCP range:

`10.10.60.50 – 10.10.60.240`

Guest devices use an external DNS service rather than an internal DNS server.

---

# 8. Guest Network ACL

The Guest Wi-Fi network should not be able to communicate with internal enterprise networks.

An example extended ACL could be:

```cisco
configure terminal

ip access-list extended GUEST_RESTRICTIONS

 deny ip 10.10.60.0 0.0.0.255 10.10.10.0 0.0.0.255
 deny ip 10.10.60.0 0.0.0.255 10.10.20.0 0.0.0.255
 deny ip 10.10.60.0 0.0.0.255 10.10.30.0 0.0.0.255
 deny ip 10.10.60.0 0.0.0.255 10.10.40.0 0.0.0.255
 deny ip 10.10.60.0 0.0.0.255 10.10.50.0 0.0.0.255

 permit ip 10.10.60.0 0.0.0.255 any

exit

interface vlan 60
 ip access-group GUEST_RESTRICTIONS in

end
```

This policy prevents guest devices from initiating connections to internal VLANs while allowing traffic toward external destinations.

---

# 9. Management VLAN ACL

Management interfaces should only be accessible from authorized administrative systems.

For example, assume an authorized IT administration workstation uses:

`10.10.30.10`

An example ACL could be:

```cisco
configure terminal

ip access-list extended MANAGEMENT_ACCESS

 permit ip host 10.10.30.10 10.10.10.0 0.0.0.255
 deny ip any 10.10.10.0 0.0.0.255
 permit ip any any

exit
```

This demonstrates how access to the Management VLAN could be restricted to an approved administrative workstation.

---

# 10. Basic Switch Security

Unused switch ports should be disabled.

```cisco
configure terminal

interface range GigabitEthernet1/0/10-23
 description UNUSED_PORT
 shutdown

end
```

Additional access-port protections could include:

```cisco
interface GigabitEthernet1/0/1

 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict

 spanning-tree portfast
 spanning-tree bpduguard enable
```

These controls can reduce the risk associated with unauthorized devices and unexpected Layer 2 activity.

---

# 11. Secure Remote Management

SSH should be used instead of Telnet for remote network administration.

Example:

```cisco
configure terminal

ip domain-name enterprise.local

username netadmin privilege 15 secret <SECURE_PASSWORD>

crypto key generate rsa modulus 2048

ip ssh version 2

line vty 0 4
 transport input ssh
 login local

end
```

> **Note:** Real credentials should never be stored in a public GitHub repository. The placeholder above intentionally avoids exposing a password.

---

# 12. Verification Commands

After configuration, network administrators can verify operation using commands such as:

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show access-lists
show mac address-table
show spanning-tree
show ip dhcp binding
```

These commands can help confirm VLAN membership, trunk operation, Layer 3 interfaces, routing, ACLs, MAC address learning, spanning-tree status, and DHCP assignments.

---

# Configuration Summary

These examples demonstrate how the logical enterprise network design could be translated into network device configurations.

The examples cover:

- VLAN creation
- Layer 3 SVIs
- Inter-VLAN routing
- Access ports
- 802.1Q trunking
- DHCP services
- Guest network isolation
- Management access restrictions
- Basic switch-port security
- Secure remote administration
- Configuration verification

The configurations are intentionally vendor-specific examples used to demonstrate networking concepts within a simulated environment.
