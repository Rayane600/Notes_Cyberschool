
---

Galmel Yann
Jelidi--Daniel Rayane

---
### Part I: Playing with DNS

**Question 1:**  
Local resolver :   `cat /etc/resolv.conf`. 

**Question 2:**  
Running `dig A istic.univ-rennes1.fr`  show details about the DNS resolution for that domain, including the IP address.

**Question 3:**  
`dig +trace +nodnssec A istic.univ-rennes1.fr` performs a full recursive DNS query. This command is showing the path taken through DNS servers to resolve a domain name. Some ipv6 addresses are unreachable maybe because the university infrastructure isn't well equipped for ipv6

**Question 4:**  
Running `dig A google.fr` multiple times shows a varying number before the IP, which represents the Time To Live (TTL) value. TTL indicates how long a DNS record is cached before rechecking with the server. and the authority section is not in the same order

**Question 5:**  
- `dig SOA univ-rennes1.fr` shows the Start of Authority record, providing information about the primary DNS server for the domain.
- `dig NS univ-rennes1.fr` lists the Name Servers for the domain.
- `dig MX univ-rennes1.fr` retrieves Mail Exchanger records.
- `dig +short istic.univ-rennes1.fr` returns a concise result, useful for scripts.

**Question 6:**  
Reverse DNS (`dig -x 129.20.126.134`) resolves an IP address to its domain name, using a PTR record. The request is constructed by reversing the IP and adding `.in-addr.arpa`. return `nfrontaldrupal.univ-rennes.fr.`

**Question 7:**  
The `@address` in `dig` specifies the DNS server to query directly. `AXFR` requests a zone transfer, which an attacker could misuse to view DNS records. Attackers may determine the first server to query by analyzing DNS records or using brute-force methods.

**Question 8:**  
The provided script scans an IP range for PTR records. Attackers could use it for network mapping. It is more precise than `AXFR` because it doesn’t rely on DNS servers permitting full zone transfers.

**Question 9:**  
`dig AXFR . @f.root-servers.net` tries to fetch the root zone file. The output  reveals the structure of global DNS.

**Question 10:**  
- **NSEC:** Indicates non-existent DNS records.
- **RRSIG:** Contains digital signatures for DNSSEC records.
- **DS:** Links DNS zones, securing delegations in DNSSEC.

### Part II: Build Your Own DNS Server

**Question 11:**  
The code snippet sends a DNS A record request over UDP to `9.9.9.9`, a public DNS server.
```python
#!/bin/env python3
import dns
import dns.message
import socket
DNS_RESOLVER = ("9.9.9.9", 53)  # Target DNS server and port
# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
req = dns.message.make_query(input(), "A")  
print("............................................")
print(req.to_text())
print("............................................")
print("You can compare it to wireshark: ")
print(req.to_wire())
print("............................................")
sock.sendto(req.to_wire(), DNS_RESOLVER)
data, addr = sock.recvfrom(1024)
resp = dns.message.from_wire(data)
print(resp)
sock.close()
print("............................................")

```

**Question 12:**  
The Python script initializes a DNS server locally, listens for DNS queries, and responds with `SERVFAIL`.

**Question 13:**  
```python 
#!/bin/env python3

import dns
import dns.message
import socket

# Local server configuration
DNS_LOCALRSL = ("127.0.0.1", 53)

# External DNS resolver to forward requests to (for example, 8.8.8.8, Google Public DNS)
DNS_FORWARDER = ("8.8.8.8", 53)

# Set up a UDP socket for the local DNS server
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(DNS_LOCALRSL)

print("Server is listening on 127.0.0.1:53")
print("Use `dig example.org @127.0.0.1` to test")

try:
    while True:
        # Receive a DNS query from the client
        data, client_addr = s.recvfrom(1024)
        print(f"Received request from {client_addr}")

        # Parse the incoming DNS query
        req = dns.message.from_wire(data)
        print("Request received:", req)

        # Set up a socket to forward the request to the external DNS resolver
        with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as forward_sock:
            # Send the client's query to the external DNS resolver
            forward_sock.sendto(data, DNS_FORWARDER)

            # Receive the response from the external DNS resolver
            resp_data, _ = forward_sock.recvfrom(1024)

            # Parse and print the forwarded response (optional for debugging)
            resp = dns.message.from_wire(resp_data)
            print("Response from forwarder:", resp)

            # Send the response back to the client
            s.sendto(resp_data, client_addr)
            print(f"Response sent back to {client_addr}")

except KeyboardInterrupt:
    print("Shutting down server...")

finally:
    s.close()
    print("Server closed.")

```

**Question 14:**  
Recording traffic in Wireshark during `dig @qlf-doh.inria.fr +https istic.univ-rennes1.fr` reveals DNS requests. It  leaks details like queried domains.

**Question 15:**  
The script performs a DNS-over-HTTPS (DoH) request. Wireshark captures an HTTPS connection, hiding traditional DNS details.

**Question 16:**  
```python
#!/bin/env python3

import dns
import dns.message
import socket
import urllib.request
import json
DNS_LOCALRSL = ("127.0.0.1", 53)

DOH_SERVER_URL = "https://cloudflare-dns.com/dns-query"

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(DNS_LOCALRSL)

print("DoH Proxy DNS Server is listening on 127.0.0.1:53")
print("Use `dig example.org @127.0.0.1` to test")

def forward_to_doh(dns_query_data):
    """Forwards the DNS query to a DoH server and returns the response."""
    
    req = dns.message.from_wire(dns_query_data)

    query_wire = req.to_wire()
    
    headers = {
        "Content-Type": "application/dns-message", 
        "Accept": "application/dns-message"
    }
    http_req = urllib.request.Request(
        DOH_SERVER_URL, 
        data=query_wire, 
        headers=headers,
        method="POST"
    )
    try:
        with urllib.request.urlopen(http_req) as response:
            doh_response_data = response.read()
            return doh_response_data
    except Exception as e:
        print("Error contacting DoH server:", e)
        return None

try:
    while True:
        data, client_addr = s.recvfrom(1024)
        print(f"Received DNS query from {client_addr}")
        doh_response_data = forward_to_doh(data) 
        if doh_response_data:
            s.sendto(doh_response_data, client_addr)
            print(f"Sent response back to {client_addr}")
        else:
            print("Failed to retrieve response from DoH server")
except KeyboardInterrupt:
    print("Shutting down server...")
finally:
    s.close()
    print("Server closed.")
```

**Question 17:**  
Limits of this approach include bypassing local DNS caching mechanisms and potential complications handling DoH errors.