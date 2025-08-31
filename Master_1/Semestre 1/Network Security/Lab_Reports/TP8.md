
---

**Network Security TP 8 Report**


Galmel Yann  
Jelidi-Daniel Rayane

---

## **Part I: Simple IP Tunnel**

### Configuration:

- **Our IP:** `148.60.12.85`
- **Neighbor IP:** `148.60.12.107`
- Command used for setting up the tunnel:
    
    ```bash
    ip link add ipip0 type ipip local 148.60.12.85 remote 148.60.12.107
    ip link set ipip0 up
    ip addr add 172.16.42.1/24 dev ipip0
    ```
    
- Ping test: Successfully sent ICMP packets to the neighbor using the `172.16.42.2` address.

### Question 1: Explain the protocol stack used for the ping.

The ping command uses the ICMP protocol at the Network Layer. When encapsulated through the IP tunnel, the stack becomes:

- **Application Layer:** Ping command generating ICMP requests.
- **Transport Layer:** ICMP works directly over IP. So not applicable
- **Network Layer:** Original IPv4 packet encapsulated within another IPv4 header for the tunnel.
- **Link Layer:** Ethernet frames carrying the encapsulated IP packets over the network.

---

## **Part II: TLS Tunnels with OpenVPN**

### Question 2:

- **IP Address (before VPN):** `10.41.24.27`
- **Routing Table (before VPN):**
    
    ```bash
    default via 10.41.31.254 dev wlan0 proto dhcp src 10.41.24.27 metric 600  
    10.41.24.0/21 dev wlan0 proto kernel scope link src 10.41.24.27 metric 600
    ```
    
- **Path to 8.8.8.8:**  
    Traceroute revealed a path through university routers (`10.0.7.1`, `193.51.184.26`, etc.) before reaching Google DNS.
- **Visible IP Address:** `148.60.99.7`

### Question 3:

- **IP Addresses (after VPN):**
    
    - Local IP: `10.41.24.27` (wlan0)
    - VPN IP: `10.42.0.9` (tap42)
- **Routing Table (after VPN):**
    
    ```bash
    0.0.0.0/1 via 10.42.0.1 dev tap42  
    default via 10.41.31.254 dev wlan0 proto dhcp src 10.41.24.27 metric 600  
    128.0.0.0/1 via 10.42.0.1 dev tap42  
    ```
    
    The `/1` routes divide the address space into two halves to ensure all traffic goes through the VPN.
    
- **Path to 8.8.8.8:**  
    Initial hop is the VPN gateway (`10.42.0.1`), followed by university infrastructure.
    
- **Visible IP Address:** `148.60.11.246`.
    
- **Role of `tap42`:** Acts as the virtual interface for encrypted VPN traffic.
    

### Question 4:

- **Wireshark Observations:**
    - On the **main interface (wlan0):** Encrypted packets (OpenVPN encapsulated traffic).
    - On the **VPN interface (tap42):** Decapsulated plaintext traffic (e.g., HTTP).

### Question 5:

- **Why still connected to ENT:**  
    ENT uses cookies or session tokens to maintain the login state. Changing the IP address alone does not terminate the session.

### Question 6:

**Traffic Encryption Diagram:**

```
[Client] ---encrypted---> [VPN Server] ---plaintext---> [8.8.8.8]
```

Only the traffic between the client and the VPN server is encrypted. Traffic from the VPN server to the destination (8.8.8.8) is in plaintext.

### Question 7:

- **Ping Traffic Patterns:**  
    When we sent pings through the VPN, we noticed that the traffic had a regular, predictable pattern in terms of size and timing. This makes it possible for someone monitoring the network to guess that ICMP packets are being used, even though the actual content is encrypted. Compared to traffic from visiting a website, like the ENT portal, which is more varied and complex, pings are much easier to identify just by looking at the traffic flow.

### Question 8 :

- **VPN Server Traffic Analysis:**  
    The VPN server can see the size and timing of packets passing through, which reveals traffic patterns. For example, a ping generates small, regular packets, while website visits produce larger, irregular packets. This means that someone monitoring the server could potentially guess what kind of activity we’re doing, even if the content is encrypted. So VPNs can’t provide full anonymity.

#### Traffic Flow Diagram:

```
[Client]
    |
    |---> Small, regular packets (encrypted)
    |     (e.g., ICMP ping traffic)
    |
[VPN Server]
    |
    |---> Small, regular packets (plaintext,same size/pattern)
    |     (Traffic forwarded to destination: 8.8.8.8)
    |
[Destination]
```

For a web request:

```
[Client]
    |
    |---> Larger, irregular packets (encrypted)
    |     (e.g., HTTP/HTTPS traffic)
    |
[VPN Server]
    |
    |---> Larger, irregular packets (plaintext)
    |     (Forwarded to web server, revealing traffic patterns)
    |
[Web Server]
```

### Question 9:
```bash
#!/bin/bash
VPN_INTERFACE="tap42"
ALLOWED_DESTINATIONS=("8.8.8.8" "148.60.15.0/24")
iptables -t nat -A POSTROUTING -o $VPN_INTERFACE -j MASQUERADE

for DEST in "${ALLOWED_DESTINATIONS[@]}"; do
    iptables -A FORWARD -o $VPN_INTERFACE -d $DEST -j ACCEPT
done
iptables -A FORWARD -o $VPN_INTERFACE -j DROP
```