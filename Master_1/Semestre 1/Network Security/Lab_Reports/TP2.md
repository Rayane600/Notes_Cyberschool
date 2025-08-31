
---

Galmel Yann
Jelidi--Daniel Rayane

---

# Network Security TP2 - Report

## Question 1:
On the terminal we observe that the server shows the same messages we snet through the client.
On Wireshark's loopback interface we observe UDP packages that contain the messages we sent.

## Question 2:
We used a port > 1024 because the first 1024 ports are reserved for specific types of service (like port 80 for HTTP).

## Question 3:

We changed the UDP_PORT to use a port that isn't already in use (in this case 5005 wasn't used).

## Question 4:

Same for the last Question we just have to set the same port for the server and the client.

## Question 5:
- On the server side :
```python
#!/usr/bin/env python3
import socket
UDP_IP = "127.0.0.1"
UDP_PORT = 5005
sock = socket.socket(socket.AF_INET, # Internet
socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))
while True:
	data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
	print("received message: %s" % data)
```

we added the line :
```python
if data : 
	sock.sendto(b"reçu",addr)
```

We took the address from the data we received from the client
- On the Client side : 
```python
#!/usr/bin/env python3
import socket
UDP_IP = "127.0.0.1"
UDP_PORT = 5005
MESSAGE = b"Hello, World!"
print("UDP target IP: %s" % UDP_IP)
print("UDP target port: %s" % UDP_PORT)
print("message: %s" % MESSAGE)
sock = socket.socket(socket.AF_INET, # Internet
socket.SOCK_DGRAM) # UDP
sock.sendto(MESSAGE, (UDP_IP, UDP_PORT))
```
we added the line :
```python
data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
if data :
	print(data)
```
## Question 6: 
If the server is offline we see a Wireshark error related packet with the tag `Destination unreachable`. It is an ICMP packet with the code 3 (used for `destination unreachable`) and the message isn't sent.

## Question 7:
The impact of the `IP_RECVERR` option is that it enables the socket to receive extended error information, such as  ICMP errors that occur when sending or receiving packets.
Setting `IP_RECVERR = 1` enables this feature, while `IP_RECVERR = 0` disables it.
It doesn't change anything to the network in wireshark but makes the error appear on the client side and stops the program from looping indefinitely.

## Question 8:
The fragmentation is handled by the Network Layer (IP) 
the fields used are :
1. **Identification**: A unique ID for each packet, used to identify fragments that belong to the same  packet.
2. **Fragment Offset**: Specifies the position of the fragment in the original data.
3. **Flags**:
    - The "More Fragments" (MF) flag is set if there are more fragments after the current one.
    - The "Don't Fragment" (DF) flag can be set to not fragment the data.
4. **Total Length**: Indicates the total length of the entire IP packet, including header and data.

they are constructed based on the fragment offset and the `More fragments` flag

## Question 9:
The size of the Ethernet frames (except the last one ) are 834 bytes same for the IP packet size but since the Udp payload is 8192 bytes it indicates that the packet was fragmented.
`ip addr show` indicates a MTU of 1500 bytes which means that any packets that are larger than 1500 bytes will be fragmented .

## Question 10: 
`127.0.0.1` is a loopback address so it wouldn't be useful to observe the outer network behavior 