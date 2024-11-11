
---

Galmel Yann
Jelidi--Daniel Rayane

---

# Network Security TP1 - Report

### Part 1: Interrogating Network Setup & Infrastructure

**Q1:**  
To find the IP address of our main interface, we used the command `ip addr`. The IP is `148.60.12.192` on the interface `eth0`.

**Q2:**  
We used the command `ip route` to view the routing table. The routes displayed were:  
- `default via 148.60.12.254`  
- `148.60.12.0/24 dev eth0 proto kernel scope link src 148.60.12.192`

The first route is the default gateway, used when the destination IP is not explicitly listed in the routing table. The second route is a local route that tells the machine to handle traffic destined for the `148.60.12.0/24` network using `148.60.12.192` as the source IP.

**Q3:**  
The command `ip neigh` showed two neighbors. We pinged them and observed that pinging our neighbors took less time than pinging `google.com`. This  is because the local traffic does not need to traverse the wider network and is resolved faster.

**Q4:**  
To delete a neighbor, we used `ip neigh del 148.60.12.254 dev eth0`. The neighbor was removed from the ARP table. When pinging the address again, the ARP protocol resolved the IP address to a MAC address and the neighbor reappeared in the table.

**Q5:**  
The command `ping google.com` resolved the domain to its IP address.

**Q6:**  
- `traceroute localhost` displayed 1 hop since it is a local address.  
- `traceroute istic.univ-rennes1.fr` got stuck, likely due to firewalls or filtering. Using `sudo traceroute -T istic.univ-rennes1.fr` (TCP) or `traceroute -I` (ICMP) allowed the traceroute to complete.  
- The same issue and solution applied when tracing to `cyber.gouv.fr`.

**Q7:**  
Each machine has a different `traceroute` result due to their geographic and network locations being different, which affects the routing paths taken.


**Q8:**  
There were no major similarities in the middle of the  traceroutes.However we found some similarities in the hops that get us in and out of the university network, we can see the different routers of the university and can also catch some Rennes 1 and Renater addresses , which makes sense since they will be the ones relaying informations.

**Q9:**  
We used `ss -t -a --resolve` to list all active TCP connections with their hostnames resolved. This showed the open connections to various domains.

---

### Part 2: Monitoring and Analyzing Traffic

**Q10:**  
We connected to `http://example.org` and captured the traffic with Wireshark:
1. **Layer 2 (Data Link):** Source: Our MAC address / Destination: `d4:76:a0:53:44:e3`.
2. **Layer 3 (Network):** Source: `148.60.12.192` (our IP) / Destination: `93.184.215.14` (IP address of `example.org`).
   
Wireshark guessed the type of the Layer 3 header by inspecting the Ethernet protocol field, which indicates the encapsulated protocol (in this case, IP).

**Q11:**  
### ARP request
- **Sender MAC Address**: `50:9a:4c:82:50:7e` — this is our MAC address.
- **Sender IP Address**: `148.60.12.192` — this is our IP address.
- **Target MAC Address**: `00:00:00:00:00:00` — this indicates that the sender doesn't know the target MAC address and is asking for it.
- **Target IP Address**: `148.60.12.254` — the IP address that we are trying to resolve
- Broadcasted to all devices (MAC: ff:ff:ff:ff:ff:ff)
### ARP reply
- **Sender MAC Address**: `d4:76:a0:53:44:e3` 
- **Sender IP Address**: `148.60.12.254`
- **Target MAC Address**: `50:9a:4c:82:50:7e`
- **Target IP Address**: `148.60.12.192` 
- unicast to the sender's MAC address (`50:9a:4c:82:50:7e`)
### Adress repetition

- The **source MAC address** (`50:9a:4c:82:50:7e`) is repeated in both the Layer 2 (Ethernet) and Layer 3 (ARP) headers to indicate the sender's identity.
- In **Layer 2**, the source MAC address is necessary to ensure the packet can be routed back to the sender when a response is generated.
- In **Layer 3**, the sender's MAC and IP addresses are included so that the target machine knows who sent the request and where to send the ARP reply

**Q12:**  
1. To capture HTTP traffic on the main interface, we used:  
   ```bash
 sudo tcpdump -i <interface> tcp port 80 and host example.org -vv
   ```
   This command captures HTTP traffic on port 80.  
   
2. To save the capture to a file:  
   ```bash
sudo tcpdump -i <interface> tcp port 80 and host example.org -w capture.pcap

   ```
   This saves the captured traffic into a file named `capture.pcap`.

**Q13:**

1. To capture HTTP traffic to `gandi.mozfr.org` we used : 
```bash
sudo tshark -i <interface> -Y "http.request and ip.host == gandi.mozfr.org"
```
2. To save the capture to a file :
```bash
sudo tshark -i <interface> -Y "http.request and ip.host == gandi.mozfr.org" -w capture.pcap

```
