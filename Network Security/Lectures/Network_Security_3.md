
# Network Layer 

### Overview
- **Network Layer Services**: Responsible for moving data across the network from sender to receiver.
  
- **Key Concepts**:
  - **Forwarding**: Moving packets from one interface of a router to another.
  - **Routing**: Determining the path a packet takes from source to destination.

- **Control Plane vs Data Plane**:
  - **Data Plane**: Handles the packet forwarding on a per-router basis.
  - **Control Plane**: Manages routing logic across the network.

---

### IP Protocol
- **Datagram Format**: Includes fields like source/destination IP, TTL, and fragmentation info.
  
- **Addressing**: IP addresses are 32-bit numbers represented in dotted decimal form (e.g., 192.168.0.5).
  
- **CIDR (Classless InterDomain Routing)**: IP address ranges are represented using `/x` notation, where x is the number of bits used for the network portion of the address.
  - **Example**: 200.23.16.0/23 means the first 23 bits are used for the network, allowing up to 512 addresses in that range.

---

### Subnets and IP Fragmentation
- **Subnets**: Devices on the same subnet can communicate without passing through a router.
  - **Example**: If a company has a network 192.168.1.0/24, this could represent all devices in one office. If they split it into smaller subnets (like 192.168.1.0/26), they can isolate groups of devices.

- **IP Mask**: The subnet mask defines how much of an IP address is for the **network** and how much is for the **host**.
  - **Example**: 192.168.1.15 with a subnet mask of 255.255.255.0 (/24) means the first 24 bits are the network (192.168.1), and the last 8 bits are the host (15).

- **Fragmentation**: Large IP packets are split into smaller packets when they exceed the network's maximum transmission size (MTU).
  - **Example**: A 4000-byte packet may be fragmented into three 1500-byte packets if the MTU is 1500 bytes.

---

### Routing and Forwarding
- **Forwarding Table**: Routers use tables to decide where to send packets based on the longest prefix match.
  
- **Routing Algorithms**: These determine the best path for packets in a network.
  - **Example**: OSPF finds the shortest path, while BGP manages routing between different networks (e.g., between ISPs).

- **SDN (Software-Defined Networking)**: A centralized method for managing routing across a network.

---

### ICMP (Internet Control Message Protocol)
- **Purpose**: Used to report network errors or for diagnostic purposes.
  - **Example**: When a router can’t forward a packet, it might send an ICMP "Destination Unreachable" message.

- **Common ICMP Messages**:
  - **Echo Request/Reply**: Used by the `ping` command to check connectivity.

---

### Best-Effort Service Model
- **Internet’s Approach**: The Internet uses a "best-effort" delivery model, meaning there are no guarantees on packet delivery, timing, or order.

---
