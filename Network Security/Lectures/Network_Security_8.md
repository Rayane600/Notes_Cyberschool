### Firewalling

---

#### **What is a Firewall?**

- A system that filters network traffic to enforce security policies.
- Operates at various layers of the OSI model to control the flow of data.

**Motivation**:

- The Internet's end-to-end model allows open connectivity but also exposes private systems.
- Firewalls restrict access to:
    - Private services.
    - Sensitive devices (e.g., desktops, IoT).

---

#### **Filtering Levels**

1. **Layer 1-2 (Physical/Link)**:
    - Example: Disable unused interfaces.
2. **Layer 3 (Network)**:
    - Filter based on IP addresses.
    - Block/allow specific destinations (e.g., `www.facebook.com`).
3. **Layer 4 (Transport)**:
    - Filter by ports (e.g., allow `80/TCP`, `443/TCP`).
4. **Layer 5-7 (Application)**:
    - More resource-intensive.
    - Filters based on domain names, URLs, or specific app-layer data.

---

#### **Types of Filtering**

1. **Stateless**:
    - Filters based on packet headers only.
    - Pros:
        - Fast, low resource usage.
    - Cons:
        - Cannot track sessions or context.
    - Example: `if port == 443, accept`.
2. **Stateful**:
    - Tracks connections (e.g., `src_ip:port ↔ dst_ip:port`).
    - Pros:
        - Allows context-aware filtering (e.g., accept responses to requests).
    - Cons:
        - More resource-intensive.
        - Requires state tables.

---

#### **Firewall and NAT Relationship**

- Stateful firewalls share connection tracking with NAT.
- Example:
    - NAT translates IPs.
    - Stateful firewall enforces rules (e.g., accept responses to HTTP requests).

---

#### **Implementation**

- **Host-based**:
    - Local filtering for a single machine.
- **Network-based**:
    - Centralized on switches/routers.
    - Ideal for managing multiple devices.

**Common Tools**:

1. **Linux**:
    - **iptables**: Older, low-level.
    - **nftables**: Modern replacement for iptables.
    - **ufw**: Simplified front-end for iptables.
2. **BSD**:
    - Packet filter (`pf`).
3. **Windows/Mac**:
    - Built-in GUI-based firewalls.

---

#### **Practical Usage**

1. **Default Policies**:
    
    - Allow all outgoing traffic.
    - Deny all incoming traffic, except:
        - Responses to requests.
        - Allowed services (e.g., `HTTP`, `DNS`).
2. **Commands**:
    
    - iptables:
        - `iptables -A INPUT --src facebook.com -j DROP`.
        - `iptables -A INPUT -p tcp --sport 443 -j ACCEPT`.
    - nftables:
        - `nft add rule ip filter INPUT ip saddr facebook.com counter drop`.
        - `nft add rule ip filter INPUT tcp sport 443 counter accept`.
3. **DNS Considerations**:
    
    - DNS names resolve to IPs during rule creation.
    - Changes in DNS records are not automatically reflected in rules.

---

#### **Advanced Features**

1. **Web Caches**:
    - Proxy servers cache content to reduce latency and bandwidth.
2. **Application Firewalls**:
    - Reverse proxies filter HTTP requests.
    - Some firewalls intercept HTTPS with explicit redirection.
3. **Optimization**:
    - Use tools like `ipset` for group matching.
    - Persistent rules via `iptables-save` or `nft list ruleset`.

---

#### **Challenges**

- Rules must balance security and performance.
- High rule counts can degrade performance.
- DNS-based rules may lead to inconsistencies.

---