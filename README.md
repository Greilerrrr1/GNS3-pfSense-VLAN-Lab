# \# GNS3 VLAN Segmentation and Inter-VLAN Routing Lab (pfSense + Open vSwitch)

# 

# \## Overview

# 

# This project demonstrates the design, configuration, and validation of a segmented enterprise-style network using \*\*VLANs\*\*, \*\*Open vSwitch\*\*, and \*\*pfSense\*\* inside \*\*GNS3\*\*. The lab is split into two parts:

# 

# \* \*\*Part 1 – Network and VLAN Configuration\*\*: Building the topology, creating VLANs, configuring Open vSwitch access and trunk ports, assigning IP addresses, and implementing firewall rules.

# \* \*\*Part 2 – Traffic Capture and Validation\*\*: Using Wireshark to validate same-VLAN switching behavior versus inter-VLAN routing through pfSense.

# 

# The goal of this lab is to show a clear understanding of \*\*Layer 2 vs Layer 3 behavior\*\*, \*\*VLAN isolation\*\*, and \*\*router-on-a-stick inter-VLAN routing\*\* using industry-relevant tools.

# 

# ---

# 

# \## Tools \& Technologies Used

# 

# \* \*\*GNS3\*\* – Network simulation and topology design

# \* \*\*pfSense\*\* – Firewall and router (inter-VLAN routing)

# \* \*\*Open vSwitch (OVS)\*\* – VLAN-aware Layer 2 switching

# \* \*\*VPCS / vWorkstation\*\* – End hosts

# \* \*\*Wireshark\*\* – Packet capture and traffic analysis

# 

# ---

# 

# \## Network Topology

# 

# The final topology consists of:

# 

# \* One pfSense router

# \* Two Open vSwitch switches connected via a trunk link

# \* Two VLANs:

# 

# &nbsp; \* \*\*VLAN 1 – Software Development\*\*

# &nbsp; \* \*\*VLAN 2 – Human Resources\*\*

# 

# !\[Final Topology](images/part1-final-topology.png)

# 

# ---

# 

# \## IP Addressing Scheme

# 

# | Device         | VLAN   | IP Address   | Subnet Mask | Default Gateway |

# | -------------- | ------ | ------------ | ----------- | --------------- |

# | vWorkstation   | VLAN 1 | 172.30.0.2   | /24         | 172.30.0.254    |

# | PC3            | VLAN 1 | 172.30.0.4   | /24         | 172.30.0.254    |

# | PC2            | VLAN 2 | 172.30.1.2   | /24         | 172.30.1.254    |

# | PC4            | VLAN 2 | 172.30.1.4   | /24         | 172.30.1.254    |

# | pfSense VLAN 1 | VLAN 1 | 172.30.0.254 | /24         | —               |

# | pfSense VLAN 2 | VLAN 2 | 172.30.1.254 | /24         | —               |

# 

# ---

# 

# \## Part 1 – VLAN and Network Configuration

# 

# \### Temporary pfSense LAN Configuration

# 

# A temporary LAN IP was assigned to pfSense to allow initial access to the web configurator before VLAN segmentation was completed.

# 

# !\[Temporary pfSense LAN IP](images/part1-pfsense-temp-lan-ip.png)

# 

# ---

# 

# \### VLAN Creation in pfSense

# 

# Two VLANs were created on the LAN parent interface (`em1`):

# 

# \* VLAN 1 – Software Development

# \* VLAN 2 – Human Resources

# 

# !\[VLAN Creation](images/part1-pfsense-vlan-creation.png)

# 

# ---

# 

# \### Interface Assignments

# 

# Each VLAN was assigned to its own interface in pfSense for routing and firewall rule control.

# 

# !\[Interface Assignments](images/part1-pfsense-interface-assignments.png)

# 

# ---

# 

# \### Firewall Rules per VLAN

# 

# Firewall rules were explicitly created to allow traffic originating from each VLAN network.

# 

# \*\*VLAN 1 Firewall Rules\*\*

# 

# !\[VLAN 1 Firewall Rules](images/part1-vlan1-firewall-rules.png)

# 

# \*\*VLAN 2 Firewall Rules\*\*

# 

# !\[VLAN 2 Firewall Rules](images/part1-vlan2-firewall-rules.png)

# 

# ---

# 

# \### Open vSwitch VLAN Configuration

# 

# Open vSwitch access ports were configured with VLAN tags, while trunk links were left untagged to carry traffic for multiple VLANs.

# 

# Key commands used:

# 

# ```bash

# ovs-vsctl del-port br0 eth1

# ovs-vsctl add-port br0 eth1 tag=1

# ovs-vsctl del-port br0 eth2

# ovs-vsctl add-port br0 eth2 tag=2

# ovs-vsctl show

# ```

# 

# !\[Open vSwitch VLAN Configuration](images/part1-ovs-vlan-config.png)

# 

# ---

# 

# \## Part 2 – Traffic Capture and Validation

# 

# Wireshark was used to capture traffic between Open vSwitch and pfSense to validate correct VLAN behavior.

# 

# ---

# 

# \### Same-VLAN Traffic (Layer 2 Switching)

# 

# Traffic between devices on the same VLAN does not require routing through pfSense. Very little routed traffic is visible.

# 

# !\[Same VLAN Capture](images/part2-same-vlan-capture.png)

# 

# ---

# 

# \### Same-VLAN Traffic on VLAN 2

# 

# Communication between VLAN 2 hosts confirms correct VLAN tagging and Layer 2 forwarding.

# 

# !\[VLAN 2 Same VLAN Traffic](images/part2-vlan2-same-vlan.png)

# 

# ---

# 

# \### Inter-VLAN Traffic: Workstation to PC2

# 

# Traffic between different VLANs is routed through pfSense, confirming correct router-on-a-stick configuration.

# 

# !\[Inter-VLAN Workstation to PC2](images/part2-inter-vlan-ws-to-pc2.png)

# 

# ---

# 

# \### Inter-VLAN Traffic: PC2 to PC3

# 

# This capture validates inter-VLAN routing across both switches and pfSense.

# 

# !\[Inter-VLAN PC2 to PC3](images/part2-inter-vlan-pc2-to-pc3.png)

# 

# ---

# 

# \## Key Technical Concepts Demonstrated

# 

# \* VLAN-based network segmentation

# \* Open vSwitch access vs trunk port configuration

# \* pfSense inter-VLAN routing

# \* Layer 2 switching vs Layer 3 routing behavior

# \* Firewall rule enforcement per VLAN

# \* Packet-level traffic validation using Wireshark

# 

# ---

# 

# \## Real-World Relevance

# 

# This lab mirrors real enterprise network designs where:

# 

# \* Departments are isolated using VLANs

# \* Firewalls control inter-department communication

# \* Switches forward traffic efficiently at Layer 2

# \* Routers handle inter-VLAN routing securely

# 

# ---

# 

# \## Author

# 

# Greiler Abrahantes

# 

# Cybersecurity \& IT Student

# 

# ---

# 

# \## Repository Structure

# 

# ```

# ├── README.md

# ├── images/

# │   ├── part1-final-topology.png

# │   ├── part1-ovs-vlan-config.png

# │   ├── part1-pfsense-temp-lan-ip.png

# │   ├── part1-pfsense-vlan-creation.png

# │   ├── part1-pfsense-interface-assignments.png

# │   ├── part1-vlan1-firewall-rules.png

# │   ├── part1-vlan2-firewall-rules.png

# │   ├── part2-same-vlan-capture.png

# │   ├── part2-vlan2-same-vlan.png

# │   ├── part2-inter-vlan-ws-to-pc2.png

# │   └── part2-inter-vlan-pc2-to-pc3.png

# ```



