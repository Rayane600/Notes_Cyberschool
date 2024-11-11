
---

Galmel Yann
Jelidi--Daniel Rayane

---
# Network Security TP3 - Report

# Part I.A

## Question 1.1.

`netcat -l -p 10000`

## Question 1.2.

`netcat localhost 10000`

## Question 1.3.

With TCP there is an ACK and SEQ in the packets , since TCP must guarantee packet delivery.

## Question 1.4.
A TCP exchange contains the three-way handshake , SYN,SYN-ACK and ACK
at the end of the exchange we see FIN and FIN-ACK
```
[Client] -> SYN -> [Server]
[Server] -> SYN-ACK -> [Client]
[Client] -> ACK -> [Server]
[Client] -> DATA -> [Server]
[Server] -> ACK -> [Client]

```
## Question 1.5.
**SEQ and ACK numbers in TCP exchange:**
- **SEQ**numbers track the position of the first byte in the data being sent
- **ACK** numbers indicate the next byte expected from the sender.
in our exchange :
- The client sends data starting at **SEQ = 1**, and the server acknowledges it with **ACK = 1**.
- In one of the frames, the client sends the **FIN** flag with **SEQ = 26**, marking the end of its data transmission, which the server acknowledges in the nextFrame.

# Part I.B

## Question 2.1.

We changed the TCP_PORT to use a port that isn't already in use (in this case 5005 wasn't used).
TCP requires connection establishment and ensures delivery. UDP on the contrary sends data without any connection and without delivery guarantee.
## Question 2.2.

Same for the last Question we just have to set the same port for the server and the client.
In TCP, once a connection is accepted, the server communicates only with the connected client where UDP can handle multiple clients without connections
## Question 2.3.
In the current Python TCP server implementation, the server will only accept and handle one connection at a time. This is because `conn` is reused for a single client-server communication.
The server can only accept one client connection, process it, and then move to the next one after the first connection is closed. This creates a delay between clients, as each client must wait until the previous one is done.
To fix this we could think about implementing multi-threading to create a new thread to handle each new client.

## Question 2.4.
**TCP behavior**: With TCP, packet loss is detected, and re-transmissions will occur. that explains the duplicated packets and the packets highlighted in red (re-transmissions) 
**UDP behavior**: We do not observe re-transmissions or duplicated packets in UDP lost packets are just lost and there is no way to get a hold of them 

## Question 2.5.
**TCP behavior** : packets are received in order. there is sometimes re-transmissions highlighted in wireshark 
**UDP behavior** : On top of the losses of packets, we can see that some packets are delivered out of order.  

## Question 2.6.
TCP handles fragmentation by breaking large data into smaller segments based on the MTU. Each segment is numbered and reassembled at the receiver, ensuring reliable delivery even if fragmentation occurs.

## Question 2.7.
TCP re-transmits missing fragments to ensure complete reassembly, handling loss more robustly. UDP, however, discards the entire data if any fragment is lost, as it doesn't have  re-transmission mechanisms.