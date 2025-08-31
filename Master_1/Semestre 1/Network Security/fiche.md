Networking Layers  
Physical: Bit transmission over medium. Protocols: Ethernet, Wi-Fi (802.11).  
Data Link: Framing, MAC addressing, error detection (CRC). Protocols: Ethernet, ARP.  
Network: Routing, addressing (IP). Protocols: IPv4, IPv6, OSPF, BGP. NAT for private-public translation.  
Transport: TCP (connection-oriented, reliable), UDP (connectionless, fast). Ports: TCP/80 (HTTP), UDP/53 (DNS).  
Application: HTTP, DNS, SMTP, FTP, IMAP, SSH.
DHCP (Dynamic Host Configuration Protocol)  
Assigns IPs dynamically. Steps: Discover → Offer → Request → ACK.  
Advantages: Plug-and-play networking, address reuse, mobility. Provides gateway, DNS info
NAT (Network Address Translation): Maps internal private IPs (RFC 1918: 192.168.x.x, 10.x.x.x) to public IPs.  
Advantages: Conserves IPv4 space, adds basic security.  
Disadvantages: Breaks end-to-end communication, introduces latency.  
Types: Static NAT (1:1), Dynamic NAT (many:many), PAT (many:1).
DNS (Domain Name System)  
Resolves names ↔ IPs. Hierarchy: Root → TLD (.com) → Authoritative.  
Record Types: A (IPv4), AAAA (IPv6), MX (mail), CNAME (alias), TXT (misc info).  
DNSSEC: Adds cryptographic signatures for integrity.
HTTP/HTTPS  
HTTP: Stateless. Methods: GET, POST, PUT, DELETE. HTTPS: HTTP over TLS (port 443). Cookies manage sessions. Caching speeds up performance.  
HTTP/2: Multiplexing, header compression. HTTP/3: Runs over QUIC (UDP).
Firewalling  
Controls traffic. Stateless: Inspects each packet independently. Stateful: Tracks active connections. Layer 7: Filters based on app data (e.g., URL).  
Policies: Allow, Block, Log. DMZ isolates public-facing servers.
TLS/SSL (Transport Layer Security)  
Secures communication (auth, encryption, integrity). TLS 1.3: Faster, mandatory Perfect Forward Secrecy (DHE/ECDHE).  
Attacks: BEAST (CBC chaining), CRIME (compression side-channel), RC4 biases (weak keystream).
IPv4 vs IPv6  
IPv4: 32-bit (4.3 billion addresses). Workarounds: NAT, CIDR.  
IPv6: 128-bit. Address types: Unicast (1:1), Multicast (1:many), Anycast (1:nearest).  
Address Format: 2001:db8::1.
Routing Protocols  
Static Routing: Manually configured.  
Dynamic Routing: Adapts to network changes. IGPs: RIP, OSPF. EGPs: BGP.
TCP vs UDP  
TCP: Reliable, ordered. Flags: SYN (start), ACK, FIN (close). UDP: Lightweight, connectionless. Example: DNS, VoIP, streaming.
IP Subnetting  
Subnet Mask: Splits IP into network/host. Example: /24 = 255.255.255.0.  
CIDR: Aggregates IPs to reduce routing tables. Example: 192.168.0.0/22.
Common Ports  
20/21 (FTP), 22 (SSH), 25 (SMTP), 53 (DNS), 80 (HTTP), 443 (HTTPS).
Cryptography Basics  
Goals: Confidentiality (AES), Integrity (HMAC), Authenticity (RSA). Hashing: Fixed-size output (SHA-256). Symmetric: AES. Asymmetric: RSA, ECC.
TLS Key Exchange  
RSA: Server sends encrypted premaster secret. DHE/ECDHE: Perfect Forward Secrecy; ephemeral keys per session.
Network Attacks  
MITM: Intercept comms. Defense: TLS, HSTS. DoS/DDoS: Overload target. Defense: Rate limiting. IP Spoofing: Forge source IP. Defense: Filtering. DNS Spoofing: Poison cache. Defense: DNSSEC.
QoS (Quality of Service)  
Prioritizes traffic (e.g., VoIP over HTTP). Methods: Traffic shaping, queue mgmt.
Miscellaneous  
ARP: Maps IP ↔ MAC. Vulnerable to spoofing. ICMP: Diagnostics (ping, traceroute). VLAN: Isolates networks within the same LAN.