
---
### Layer 2 Analysis:
- **Header:** Consists of destination address, source address, and EtherType.
- **Destination Address:** `00 00 cc f3 a5 d2`
- **Source Address:** `0c 58 91 cf 52 75`
- **EtherType:** `08 00` (IPv4)
- **Payload:** Data following the EtherType, varying from 46 to 1500 bytes.
- **CRC:** Last 4 bytes, used for error detection.

---
#### CRC Visibility in Frame Analysis

When analyzing network frames at the software level, the **CRC (Cyclic Redundancy Check)** is not visible. This is because:
- The **CRC is handled by the network interface card (NIC)**, which computes it during transmission and checks it upon reception.
- **Once a frame is received**, the NIC removes the CRC before passing the frame to the operating system or capturing tools like Wireshark.
- **CRC is only used for error detection at the link layer** and is not passed to higher layers or included in software-level analysis.
---
### Layer 3 Analysis:
- **Protocol Used:** EtherType `08 00` indicates IPv4.
- **Appletalk Reference:** Listed with a name/email because it's outdated and rarely used now. It serves as a historical reference.

---

### Another Packet Analysis:
- **Header:** Contains destination address, source address, and EtherType.
- **Destination Address:** `ff ff ff ff ff ff` (broadcast address)
- **Source Address:** `58 91 cf 52 75 bd`
- **EtherType:** `08 00` (IPv4)
- **Payload:** Data starts after the EtherType.
- **CRC:** Not included in provided data.
- **Broadcast Address Role:** The `ff ff ff ff ff ff` destination address means the packet is sent to all devices on the network.

---

### ARP Protocol:
- **A’s ARP Table:**

| Host | Address           | TTL |
| ---- | ----------------- | --- |
| B    | BB:11:22:33:44:55 | 60  |

- **Source & Destination for A to Reach B:**
  - Source: A’s MAC `AA:11:22:33:44:55`
  - Destination: B’s MAC `BB:11:22:33:44:55`
- **TTL Role:** Limits packet’s lifetime; decremented by routers. Packet is discarded if TTL reaches 0.

---

### Updating Table:
- **A Sending a Packet to C:** A performs ARP to resolve C’s MAC. After resolving, A sends the packet with C’s MAC address as the destination.
**Source & Destination for A to Reach C:**
	1- Step 1 : 
		- Source: A’s MAC `AA:11:22:33:44:55`
		- Destination: Broadcast `FF:FF:FF:FF:FF:FF`
	 2- Step 2 :
		- Source: C's MAC `CC:11:22:33:44:55`
		- Destination: A’s MAC `AA:FF:FF:FF:FF:FF`
	 3- Step 3 :
		- Source: A's MAC `AA:11:22:33:44:55`
		- Destination: C’s MAC `CC:FF:FF:FF:FF:FF`
- **B Impersonating C:** B could perform ARP spoofing, tricking A into sending data intended for C to B.

---

### Switch Table:

| Address           | Port | TTL |
| ----------------- | ---- | --- |
| AA:11:22:33:44:55 | 1    | 60  |
| BB:11:22:33:44:55 | 2    | 60  |
|                   |      |     |
