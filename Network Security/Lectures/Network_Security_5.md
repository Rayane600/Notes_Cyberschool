### NAT and DHCP

---

#### **Network Address Translation (NAT)**

**Overview:**

- Allows multiple devices in a private network to share a single public IP address.
- Operates by modifying packet headers to map private to public IPs.

**Key Features:**

1. **Private IPs:**
    - Devices use private IPs (10.x.x.x, 192.168.x.x, 172.16.x.x).
    - Invisible to the external network.
2. **Public IP Translation:**
    - NAT assigns one public IP for communication with external networks.
    - Modifies source/destination IP and port numbers in packets.

**Types of NAT:**

- **Source NAT:** Changes the source IP of outgoing packets (most common).
- **Destination NAT:** Changes the destination IP, often for hosting servers.
- **Carrier-Grade NAT (CG-NAT):** Used by ISPs to manage large-scale networks.

**Advantages:**

- Conserves IPv4 addresses.
- Offers basic security by hiding internal network details.
- Flexibility in managing private IP changes.

**Controversies:**

- Breaks the end-to-end connectivity model.
- Challenges for protocols like VoIP or P2P (requires NAT traversal techniques).

**NAT & Security:**

- NAT inherently blocks unsolicited incoming connections.
- However, it is not a firewall; filtering is a side effect.
- Vulnerabilities:
    - Port forwarding opens access points.
    - Techniques like UPnP and hole-punching bypass NAT.

---

#### **Dynamic Host Configuration Protocol (DHCP)**

**Overview:**

- Automates IP address assignment for devices joining a network.
- Centralized server handles configuration to reduce manual effort.

**How It Works:**

1. **Discovery:** Host sends a broadcast message to locate a DHCP server.
2. **Offer:** DHCP server responds with an available IP and configuration.
3. **Request:** Host requests the offered IP.
4. **Acknowledgment:** Server confirms and assigns the IP with additional data.

**Advantages:**

- Supports mobile devices joining/leaving networks dynamically.
- Reuses IPs for efficient management.
- Can provide additional settings like:
    - Subnet mask.
    - Gateway (first-hop router).
    - DNS server.

**Security Concerns:**

- Broadcast nature of DHCP makes it vulnerable to spoofing attacks.
- Rogue DHCP servers can mislead devices.
- **Mitigation:** Use managed switches with DHCP snooping to filter legitimate servers.

**Alternatives:**

- Static IP configuration (manual).
- ICMP Router Advertisement (IPv6).
- Self-assigned IPs (169.254.x.x) for ad hoc networking.

---