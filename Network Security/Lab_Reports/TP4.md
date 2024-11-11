
---

Galmel Yann
Jelidi--Daniel Rayane

---
### **Part I: First Hands with Scapy**

#### **Question 1: Discuss what you observe**

**Steps and Commands:**

1. **Launch Scapy:**
   ```bash
   sudo scapy
   ```
   You will now be in the Scapy Python shell.

2. **Explore IP packet structure:**
   ```python
   IP().show()
   ```
   We  observed fields such as `version`, `ihl`, `tos`, `len`, `id`, `ttl`, `proto`, `src`, and `dst`.

3. **Create a packet with a specific source IP and inspect:**
   ```python
   pkt = IP(src="192.0.2.1")
   pkt.show()
   ```
   The `src` field is modified to `192.0.2.1`.

4. **Build more complex packet layers:**
   ```python
   (IP()/ICMP()).show()
   (IP()/UDP()/Raw(b"Hello")).show()
   ```
   These commands construct a simple ICMP packet and a UDP packet with raw payload data ("Hello").  Scapy builds the layers of packets.

- Fields in the packet headers are either default or customized.
- Scapy is able to combine layers (IP/UDP) without worrying about   checksum or length .

#### **Question 2: Sending our first forged packets**

**Steps and Commands:**

1. **Create and send a ping request:**
we used `ip route` to find the router address => default via 
   ```python
   req = IP(dst="148.60.12.254")/ICMP() 
   send(req)  # No need to wait for a reply
   ```

2. **Send and receive (wait for a response):**
   ```python
   resp = sr1(req)  # sr1 sends one packet and waits for one reply
   resp.show()
   ```
   If we launch Scapy without root privileges, we had permission errors. 
   - Always run Scapy with `sudo`.

- We observe  an ICMP packet sent, and when the router replies, we'll get an ICMP response (pong).

#### **Question 3: Traceroute**


**Steps and Commands:**

1. **Initialize the packet:**
   ```python
   p = IP(dst="8.8.8.8", ttl=(1,30))/ICMP()  
      ```

2. **Send and receive the packet:**
   ```python
   resp= sr1(p, timeout=1, verbose=0)
   resp.show()
   ```

**What to observe:**
- As the TTL (time to live) expires, each router along the path sends back a "TTL expired" message, allowing you to trace the route. if we put TTL =1 we get the router then TTL =2 gets us on one of the internal servers of the DSI and so on .

#### **Question 4: Spoofing**

**Steps and Commands:**

1. **Create a spoofed packet:**
   ```python
   pkt = IP(src="148.60.12.161", dst="8.8.8.8")/ICMP()  
   req.ttl = 150
   send(pkt)
   ```

2. **Observe using Wireshark:**  
   We ran Wireshark on the receiving machine and observed the incoming packet, noting that the source IP is the spoofed one .

**What to observe:**
- The reply (pong) will not return to us but will be sent to the spoofed IP.

---

### **Part II: Scapy for UDP**

#### **Question 5: Interact with your UDP client & server**

**Steps and Commands:**

1. **UDP packet creation:**
   ```python
   pkt = IP(dst="127.0.0.1")/UDP(dport=12345)/Raw(b"Hello")
   send(pkt)
   ```

2. **Server listening:**
   We made sure our UDP server was running on port `12345`. Use `netcat` for testing:
   ```bash
   nc -lu 12345
   ```
   We saw "Hello" on the server side.

#### **Question 6: Triggering an infinite loop**
**This question was not done in the lab so it wasn't tested directly**
We could craft a packet that triggers an infinite loop in our UDP server by sending packets that the server can’t handle correctly.

**Steps and Commands:**

1. **Sending a malformed packet:**  
   Sending an unexpected packet structure that the server isn’t prepared to handle, which could cause an issue.

2. **Mitigation:**  
   To prevent this, We have to ensure our server checks packet content before responding.

---

### **Part III: Scapy for TCP**

#### **Question 7: Sending TCP data**

Here, you’ll send TCP data to your server using Scapy.

**Steps and Commands:**

1. **TCP packet creation and sending:**
   ```python
   pkt = IP(dst="127.0.0.1")/TCP(dport=12345)/Raw(b"Hello")
   send(pkt)
   ```

#### **Question 8: Configuring TCP**

We need to use the `TCP()` layer in Scapy for sending TCP packets and we have to make sure our server listens on a TCP port.

#### **Question 9: Spoofing a TCP client**

we can spoof a TCP client and try to perform actions like opening a connection. However, this is detectable due to the TCP handshake .

**Steps and Commands:**

1. **Spoofing a connection:**
   ```python
   syn = IP(src="1.2.3.4", dst="192.168.1.1")/TCP(dport=12345, flags="S")
   send(syn)
   ```
   Spoofing TCP is tricky due to sequence numbers and the handshake, so it's often detected.

#### **Question 10: Port scanning**



**Steps and Commands:**

1. **TCP SYN scan:**
   ```python
   def scan_port(dst_ip, dst_port):
       pkt = IP(dst=dst_ip)/TCP(dport=dst_port, flags="S")
       resp = sr1(pkt, timeout=1, verbose=0)
       if resp and resp.haslayer(TCP) and resp.getlayer(TCP).flags == "SA":
           return True
       return False

   for port in range(1, 1024):
       if scan_port("192.168.1.1", port):
           print(f"Port {port} is open")
   ```

#### **Question 11 : Responding with Scapy**

**Steps and Commands:**

1. **Sniff and respond:**
   ```python
   def reply(pkt):
       if pkt.haslayer(UDP):
           send(IP(dst=pkt[IP].src)/UDP(dport=pkt[UDP].sport)/Raw(b"Response"))

   sniff(filter="udp dst port 12345", prn=reply)
   ```

---
**Note : Some of the answers on this report were given out of simple internet research without direct testing since we didn't have time to finish the lab in class **