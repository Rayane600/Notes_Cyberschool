
---

# Transport Layer Overview

## Key Concepts
- **Transport Layer Services**:
  - Multiplexing & demultiplexing
  - Reliable data transfer
  - Flow control
  - Congestion control

- **Internet Transport Protocols**:
  - **UDP**: Connectionless, best-effort delivery.
  - **TCP**: Connection-oriented, reliable delivery with flow and congestion control.

## Example
### Client-Server Paradigm
- **Server**: Always-on, fixed IP, located in data centers.
- **Client**: Connects to the server, dynamic IPs, no direct communication with other clients.

---

# Multiplexing & Demultiplexing

## Concept
- **Multiplexing**: Transport layer collects data from multiple sockets, adds transport headers, and sends them to the network.
- **Demultiplexing**: Receives data segments, uses header information to deliver segments to the correct socket.

## Example
- A UDP server socket listens on port 6428, and clients send datagrams to this port. The transport layer uses the destination port to direct the data to the correct application process.

---

# UDP: User Datagram Protocol

## Key Features
- **Connectionless Protocol**: No need for a handshake between sender and receiver.
- **Unreliable**: Segments might be lost or delivered out of order.
- **Applications**: Streaming, DNS, HTTP/3, gaming.

## Example
- **Streaming Applications**: Video and audio streaming use UDP because it is faster, and they can tolerate some data loss.

## UDP Header Structure
- **Source Port**
- **Destination Port**
- **Length**: Length of UDP segment, including header.
- **Checksum**: Error detection mechanism for data integrity.

---

# TCP: Transmission Control Protocol

## Key Features
- **Reliable, In-order Delivery**: TCP ensures that data arrives correctly and in the proper order.
- **Connection Management**: Uses a three-way handshake to establish a connection.
- **Flow Control**: Prevents sender from overwhelming receiver using window size.
- **Congestion Control**: Avoids network congestion by adapting the sending rate.

## Example
### Three-Way Handshake
1. **Client** sends a SYN (synchronize) message.
2. **Server** responds with a SYN-ACK (synchronize-acknowledge) message.
3. **Client** replies with an ACK to establish the connection.

## TCP Header Structure
- **Sequence Number**: Used to order bytes.
- **Acknowledgment Number**: Indicates the next byte expected.
- **Flags**: SYN, ACK, FIN for connection management.

---

# Flow and Congestion Control

## Flow Control
- **Receive Window (rwnd)**: The amount of data the receiver is willing to accept. Prevents buffer overflow.

## Congestion Control
- **Additive Increase, Multiplicative Decrease (AIMD)**: Gradually increase sending rate until loss, then cut in half to reduce congestion.
- **Example**: TCP reduces the sending rate when packet loss is detected, adjusting to network conditions.

---

# Additional Topics

## TCP vs. UDP
- **TCP** is used for reliable communication such as **HTTP, FTP**, and **SMTP**.
- **UDP** is preferred for latency-sensitive applications like **streaming** and **gaming**.

## Connection Termination
- TCP connections are closed by exchanging FIN and ACK messages from both ends.

## Other Protocols
- **QUIC**: Designed to supplement or replace TCP/UDP, providing faster connection establishment (0-RTT).

---
