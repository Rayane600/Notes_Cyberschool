### Domain Name System (DNS)

---

#### **What is DNS?**

- The "Internet Phone Book" for translating human-readable domain names to IP addresses (IPv4/IPv6).
- Core functions include:
    - **Name-to-IP translation** (e.g., `google.com` → IP address).
    - **Reverse DNS**: IP-to-Name resolution.
    - Supporting other features like:
        - **Email routing** (MX records).
        - **Cryptographic exchanges** (DNSSEC, TLSA).
        - **Load balancing** (Round Robin, GeoDNS).

---

#### **DNS Architecture**

- **Distributed Database**: Organized hierarchically as a tree:
    1. **Root (`.`)**: Managed by ICANN and operated by 13 root server clusters.
    2. **Top-Level Domains (TLDs)**:
        - Generic (gTLD): `.com`, `.org`.
        - Country-specific (ccTLD): `.uk`, `.fr`.
        - Sponsored (sTLD): `.gov`, `.edu`.
    3. **Domains**: Managed by registrars and delegated by TLD operators.
    4. **Subdomains**: Created and controlled by domain owners.

---

#### **DNS Resolution**

- Iterative process for resolving domain names:
    1. Query root servers for TLD information.
    2. Query TLD servers for domain information.
    3. Query domain's authoritative server for specific records.
- **Caching**:
    - DNS responses have TTLs to improve performance.
    - Cached records expire based on TTL (e.g., `300s` for `www.google.com`).

---

#### **Key Record Types**

- **A**: Maps domain to IPv4 address.
- **AAAA**: Maps domain to IPv6 address.
- **NS**: Specifies authoritative nameservers.
- **MX**: Indicates mail servers for a domain.
- **CNAME**: Alias for another domain name.
- **TXT**: Stores arbitrary text (e.g., SPF, DKIM for email security).

---

#### **Transport Protocols**

- DNS operates over UDP/53 (default) for speed but uses TCP/53 for reliability and larger responses.
- Secure alternatives:
    - **DNS over TLS (DoT)**.
    - **DNS over HTTPS (DoH)**.
    - **DNS over QUIC (DoQ)**.

---

#### **DNS Security**

1. **Risks:**
    - **DDoS Attacks**: Exploit open resolvers and amplify traffic using UDP.
    - **Sniffing and MITM**: Eavesdropping or modifying DNS responses.
    - **Cache Poisoning**: Inserting malicious records into resolver caches.
2. **Mitigation:**
    - **BCP38**: Prevents IP spoofing by filtering packets with invalid source IPs.
    - **Rate-limiting** and restricting open resolvers.
    - Secure transport protocols (e.g., DoT, DoH).

---

#### **DNSSEC**

- Adds cryptographic signatures to DNS responses to prevent spoofing and cache poisoning.
- Key components:
    - **DNSKEY**: Public keys for a domain.
    - **RRSIG**: Signatures for records.
    - **DS**: Delegation signer linking parent and child zones.
    - **NSEC/NSEC3**: Proof of non-existence for unsigned zones.
- **Limitations**:
    - Does not encrypt DNS traffic (use DoH/DoT for privacy).
    - Larger DNS responses may increase DDoS risks.

---

#### **Conclusion**

- DNS is critical internet infrastructure, underpinning nearly all online activities.
- Though robust and scalable, DNS faces ongoing security and performance challenges.
- Evolves continuously with enhancements like DNSSEC and encrypted DNS transport to address modern needs.
