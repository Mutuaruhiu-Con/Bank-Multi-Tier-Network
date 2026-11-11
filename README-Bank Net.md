# Bank Network

> A Cisco Packet Tracer enterprise networking project demonstrating hierarchical network design, redundancy, routing, security, enterprise services, simulated ISP connectivity, and disaster recovery.

## Overview

This project documents a full-scale Cisco Packet Tracer simulation of a multi-site bank network. The design demonstrates a hierarchical enterprise architecture with network segmentation, redundancy, dynamic and static routing, edge services, a DMZ, simulated ISP connectivity, and a disaster recovery site.

## Network Architecture

### Access Layer
- Four Cisco 2960 access switches
- Each access switch is redundantly connected to two Layer 3 distribution switches

### Distribution Layer
- Two Cisco 3650 multilayer switches
- The distribution switches are interconnected using an EtherChannel point-to-point connection
- Both distribution switches are redundantly connected to the core

### Core Layer
- Three Layer 3 switches connected in a triangular topology
- The central core switch provides upstream connectivity toward the edge

### Network Edge
- A Cisco 2911 router connects the internal network, simulated ISP, and DMZ
- NAT/PAT and ACL-based traffic controls are implemented at the network edge

### Disaster Recovery
- A dedicated disaster recovery site is included as part of the enterprise design

## Technologies Implemented

- VLANs and Switch Virtual Interfaces (SVIs)
- Inter-VLAN routing
- OSPF dynamic routing
- Basic BGP routing
- Static routing
- Route redistribution
- NAT and PAT
- DHCP, DNS, Mail, and NTP services
- Default gateway configuration
- DMZ implementation
- Access Control Lists (ACLs)
- EtherChannel redundancy
- Disaster recovery implementation
- ISP and Internet connectivity simulation
- Hierarchical network design principles

## IP Addressing

### WAN and External Addressing

| Device | Interface | Address |
|---|---|---|
| ISP1 | G0/0 | 10.0.250.2/30 |
| ISP1 | G0/1 | 203.0.112.1/30 |
| ISP1 | G0/2 | 192.0.2.1/30 |
| ISP2 | G0/0 | 192.0.2.2/30 |
| ISP2 | G0/1 | 192.0.21.1/30 |
| ISP2 | G0/2 | 192.0.22.1/30 |
| Firewall / Edge | G0/0.1 | 172.16.10.1/24 |
| Firewall / Edge | G0/1 | 10.0.250.1/30 |
| Firewall / Edge | G0/2 | 10.0.255.2/30 |
| DR | G0/0.1 | 10.50.0.1/24 |
| DR | G0/1 | 192.0.22.2/30 |
| DR | G0/2 | 10.0.252.2/30 |

## HQ VLAN Summary

| VLAN | Department / Purpose | Subnet | Default Gateway |
|---|---|---|---|
| 10 | Finance Data | 10.13.10.0/24 | 10.13.10.1 |
| 20 | HR Data | 10.14.10.0/24 | 10.14.10.1 |
| 30 | Operations Data | 10.15.10.0/24 | 10.15.10.1 |
| 40 | IT Data | 10.16.10.0/24 | 10.16.10.1 |
| 50 | Server Farm | 10.17.10.0/24 | 10.17.10.1 |
| 60 | DMZ | 172.16.10.0/24 | 172.16.10.1 |
| 99 | Management | 10.18.99.0/24 | 10.18.99.1 |
| 100 | Disaster Recovery | 10.50.0.0/24 | 10.50.0.1 |
| 110 | Finance Voice | 10.19.110.0/24 | 10.19.110.1 |
| 120 | HR Voice | 10.20.120.0/24 | 10.20.120.1 |
| 130 | Operations Voice | 10.21.130.0/24 | 10.21.130.1 |
| 140 | IT Voice | 10.22.140.0/24 | 10.22.140.1 |

## Routing and Connectivity

- OSPF is used for dynamic routing within the enterprise environment.
- Basic BGP is used for simulated external routing within Packet Tracer capabilities.
- Static routes supplement dynamic routing where required.
- Route redistribution exchanges routing information between routing domains.

## Security and Enterprise Services

### Security
- NAT/PAT translates internal addresses for external connectivity.
- ACLs control traffic between the DMZ and internal network.
- Internal hosts can access DMZ services.
- DMZ servers are restricted from initiating connections toward the internal LAN.

### Enterprise Services
- DHCP provides automated client addressing.
- DNS provides hostname resolution.
- Mail and NTP services are included for enterprise-service testing.

## How to Run the Lab

1. Install Cisco Packet Tracer version 9.0.1 or later.
2. Clone or download this repository.
3. Open the Packet Tracer project file (`.pkt`).
4. Review the topology diagram and addressing plan.
5. Verify device configurations and interface states.
6. Run the validation tests.

## Validation Checklist

- Verify VLAN membership and trunk operation.
- Confirm required SVIs are operational.
- Test inter-VLAN connectivity.
- Verify DHCP address assignment.
- Test DNS resolution.
- Verify OSPF neighbor relationships and routing tables.
- Verify configured BGP neighbors and advertised networks supported by Packet Tracer.
- Verify route redistribution and external reachability.
- Confirm NAT/PAT translations.
- Test DMZ access controls.
- Verify EtherChannel and redundant paths.

## Troubleshooting Checklist

- Confirm access ports are assigned to the correct VLAN.
- Verify trunks permit all required VLANs.
- Confirm hosts have the correct IP address, subnet mask, and default gateway.
- Ensure SVIs are correctly configured and operational.
- Verify Router-on-a-Stick subinterfaces for the DMZ and DR implementations.
- Confirm NAT translates internal addresses as intended.
- Check routing tables and end-to-end connectivity.
- Check ACLs when expected traffic is blocked.
- Verify BGP-to-OSPF route redistribution where required.

## Problems Encountered and Lessons Learned

- Use `/30` or `/31` addressing for point-to-point links where appropriate to reduce overlap and simplify troubleshooting.
- Use `ip helper-address` on SVIs when DHCP clients and the DHCP server are in different subnets.
- The PT8200 model did not provide the BGP capability required for this lab, while the Cisco 2911 supported the demonstrated basic BGP configuration.
- Static routes may be required to achieve complete end-to-end reachability.
- The simulated mail service produced SMTP authentication failures during testing.
- NTP synchronization did not propagate to every router and switch; only three multilayer switches and the DR router synchronized at Stratum 2 during testing.
- DHCP and DNS resolution were successfully demonstrated.
- Packet Tracer supports a limited subset of BGP functionality; neighbor configuration, network advertisement, and redistribution were demonstrated, while more advanced features were unavailable.

## Platform Limitations

This project is a Cisco Packet Tracer simulation. Some production-grade protocol features and device behaviors are simplified or unavailable.

## Recommended Repository Structure

```text
Bank-Network/
├── README.md
├── packet-tracer/
│   └── Bank-Network.pkt
├── topology/
│   └── topology-diagram.png
└── docs/
```

## Future Improvements

- Add automated configuration backup and validation scripts.
- Expand monitoring and logging capabilities.
- Add additional branch-site scenarios.
- Perform more WAN resiliency testing.
- Migrate advanced components to a more feature-complete network emulator for expanded BGP functionality.

## License

No license is currently attached to this project.

## Contributions

Contributions and improvements are welcome. Potential additions include monitoring, automation, expanded validation tests, and additional enterprise services.
