
### Link Layer and LANs Overview

**Goals of the Link Layer:**
- Error detection and correction.
- Sharing a broadcast channel (multiple access protocols).
- Link layer addressing.
- Understanding LAN technologies (Ethernet, VLANs).
- Implementation of link layer technologies.

---

### Introduction to the Link Layer

- **Terminology:**
  - **Nodes**: Hosts and routers.
  - **Links**: Communication channels that connect adjacent nodes along the communication path (wired or wireless).
  - **Frame**: A Layer-2 packet that encapsulates a datagram.

- **Link Layer Responsibility**: Transferring a datagram from one node to a physically adjacent node over a link.

---

### Services of the Link Layer

- **Framing, Link Access**:
  - Encapsulates datagram into a frame, adding a header and trailer.
  - Controls channel access if the medium is shared (half/full duplex).
  - Uses MAC addresses in frame headers to identify source and destination.

- **Reliable Delivery**:
  - Provides reliable delivery between adjacent nodes (rarely used on low bit-error links, often used in wireless links).

- **Error Detection and Correction**:
  - Identifies and corrects bit errors without retransmission in limited cases.

---

### Where is the Link Layer Implemented?

- Implemented in the **Network Interface Card (NIC)** or a chip (Ethernet, WiFi card).
- Combination of hardware, software, and firmware.
- NIC attaches to the host’s system bus.

---

### Error Detection and Correction

- **EDC (Error Detection and Correction)**: Extra bits (redundancy) used to detect errors.
  - Not 100% reliable, but larger EDC fields improve error detection.

- **Cyclic Redundancy Check (CRC)**:
  - Common error-detection coding scheme.
  - Detects all burst errors smaller than the size of the CRC bits.
  - Used in Ethernet and WiFi (802.11).

---

### Multiple Access Protocols

- **Point-to-Point Links**: Direct links between two nodes.
- **Broadcast Links**: Shared wire or medium (e.g., Ethernet, WiFi).

- **Ethernet (CSMA/CD)**:
  - Detects if the channel is busy and waits for it to be idle before transmitting.
  - If a collision is detected, it sends a jamming signal and enters exponential backoff before retrying.

- **Other MAC Protocols**:
  - Time Division Multiple Access (TDMA) and Frequency Division Multiple Access (FDMA).
  - ALOHA, Bluetooth, Token Ring, and WiFi (CSMA/CA).

---

### MAC Addresses

- **IP Address (32-bit)**: Used for Layer 3 (network layer) forwarding.
- **MAC Address (48-bit)**: Used for local communication on the same subnet.
  - Unique to each interface, burned into the NIC, but can sometimes be software-settable.
  - Administered by the IEEE.

---

### ARP (Address Resolution Protocol) and ARP Tables

**ARP (Address Resolution Protocol)** is used to map a device's **IP address** (Layer 3) to its **MAC address** (Layer 2). This mapping allows devices on a local network (LAN) to communicate with each other.

#### ARP Table:

- Each device on a LAN maintains an **ARP table**, which stores the mapping between IP addresses and MAC addresses.
- The ARP table helps a device (e.g., host A) determine the **MAC address** of another device (e.g., host B) when it knows only the IP address.

**Structure of ARP Table:**

|IP Address|MAC Address|TTL|
|---|---|---|
|192.168.1.10|AA:BB:CC:DD:EE|20 min|
|192.168.1.11|00:11:22:33:44:55|20 min|

- **IP Address**: The known IP of the device.
- **MAC Address**: The MAC address of the device corresponding to the IP.
- **TTL (Time To Live)**: How long the entry remains valid (typically around 20 minutes).

#### How ARP Works:

1. **ARP Request**: If host A needs to send data to host B and does not know B’s MAC address, it sends out an ARP request as a **broadcast** on the network.
2. **ARP Reply**: Host B, upon receiving the request, responds with its MAC address in an ARP reply, which is sent directly back to host A.
3. **Table Update**: Host A then stores B’s IP and MAC address in its ARP table for future communication.

---

### Ethernet

- **Physical Topology**:
  - **Bus**: Shared collision domain (outdated).
  - **Switched**: Current, with each node having a dedicated link to a switch.

- **Frame Structure**:
  - Contains source and destination MAC addresses, type field (to identify higher layer protocol), and CRC for error detection.
  - Ethernet is **connectionless** and **unreliable** (no ACKs/NAKs from receiving NIC).

---

### Switches and Switch Tables

**Switches** are devices used in LANs to forward Ethernet frames between devices. Unlike a hub, which broadcasts all traffic to all connected devices, a switch forwards frames only to the intended recipient by learning the MAC addresses of devices connected to it.

#### Switch Table (Forwarding Table):

A switch maintains a **switch table** (or forwarding table), which maps MAC addresses to the ports on the switch where they can be reached. This table is essential for efficient communication within the network.

**Structure of a Switch Table:**

|MAC Address|Interface (Port)|TTL|
|---|---|---|
|AA:BB:CC:DD:EE|Port 1|60 sec|
|00:11:22:33:44:55|Port 2|60 sec|

- **MAC Address**: The unique address of the device.
- **Interface (Port)**: The port number on the switch where the device can be found.
- **TTL**: The time the entry remains valid in the switch table.

#### How Switches Work:

1. **Frame Reception**: When a switch receives a frame, it looks at the **source MAC address** and records the port it came from in its switch table.
2. **Forwarding Decision**: The switch then looks at the **destination MAC address**. If it is in the switch table, it forwards the frame to the correct port. If it’s not in the table, the switch floods the frame to all ports (except the one it came from).
3. **Self-Learning**: Over time, the switch learns the locations of all devices on the network by analyzing the frames and updating its switch table.

#### Example of Switch Forwarding:

1. **Host A** sends a frame to **Host B**. The switch checks its switch table:
    - If the **MAC address** of Host B is found in the table, the switch forwards the frame directly to the correct port.
    - If it’s not found, the switch floods the frame to all ports except the one where it received the frame.
2. **Host B** replies to Host A. The switch now learns the **MAC address of Host B** and its corresponding port, updating its table.

---

### VLANs (Virtual Local Area Networks)

- **Motivation**: Allows creating multiple broadcast domains without physically redoing the network cabling.
- **Port-Based VLANs**: Switches can group ports into virtual networks.
- **VLAN Trunks**: Carry frames between VLANs defined over multiple switches using the 802.1Q protocol.
