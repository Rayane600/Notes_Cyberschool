## History of Networking

- **1967:** ARPAnet created by Advanced Research Projects Agency (ARPAnet).
- **1972:** ARPAnet expands to 15 nodes.
- **1974:** Cerf & Khan propose "A Protocol for Packet Network Intercommunication".
- **1983:** Transition from NCP to TCP/IP (Flag Day).

---

## Overview of the Internet

The Internet can be viewed from different perspectives:

- A **Network of Networks**: A collection of smaller, interconnected networks, often driven by economic interests or national regulations.
- **Devices**: Includes routers, switches, antennas, computers (laptops, servers) connected through communication links like fiber optics, antennas, or cables.
- **Infrastructure**: Provides applications and services to end-users and developers. Networks are seen as "dumb" pipes where intelligence resides on the devices connected (clients and servers).
- **Protocols**: Rules and standards that allow devices and networks to communicate.

---

## Protocols

- **Protocols**: Define the format and order of messages exchanged between devices, as well as the actions taken when messages are sent or received.
- **Encapsulation**: Data is wrapped in a series of headers as it moves down the protocol stack, with each header containing information necessary for the next layer.
- **RFCs (Request for Comments)**: Documents that define Internet standards. Not all RFCs become standards; some remain informational or experimental.

---

## Layering in Internet Protocols

- **OSI Model**: The **Open Systems Interconnection model** originally had 7 layers, though it's not commonly used anymore for Internet protocols. The layers were:
    1. **Physical Layer**: Transmits raw data bits over physical media (cables, fiber optics, radio waves).
    2. **Data Link Layer**: Handles node-to-node data transfer and error detection (e.g., Ethernet, WiFi).
    3. **Network Layer**: Routes data between networks (e.g., IP protocol).
    4. **Transport Layer**: Manages the transfer of data between systems (e.g., TCP, UDP).
    5. **Session Layer**: Manages sessions and connections between applications.
    6. **Presentation Layer**: Translates data between the application and network (e.g., encryption, data compression).
    7. **Application Layer**: Interacts directly with user applications (e.g., HTTP, SMTP).
- **TCP/IP Stack**: A more practical model with 5 layers:
    1. **Application Layer**: Provides network services to applications (e.g., HTTP, DNS).
    2. **Transport Layer**: Ensures data is reliably sent and received between applications (e.g., TCP, UDP).
    3. **Network Layer**: Handles logical addressing and routing (e.g., IP).
    4. **Data Link Layer**: Transfers data between adjacent nodes (e.g., Ethernet).
    5. **Physical Layer**: Converts data into signals for transmission (e.g., electrical signals, light pulses).

---

## Security Considerations

### Attack Types

1. **Sniffing**:
    
    - **Definition**: An attacker listens to (or "sniffs") network traffic to capture sensitive information like passwords, session tokens, or unencrypted data.
    - **Example**: Tools like Wireshark are used for legitimate purposes but can be used maliciously to sniff data.
    - **Prevention**: Use encryption (SSL/TLS) to protect data in transit.
2. **Spoofing**:
    
    - **Definition**: An attacker pretends to be another entity by falsifying data to gain access or trust. Common spoofing types include **IP spoofing** and **ARP spoofing**.
    - **Example**: In **ARP Spoofing**, an attacker sends false ARP messages to associate their MAC address with the IP address of a legitimate device on the network.
    - **Prevention**: Implement packet filtering, enable authentication protocols, and use encryption.
3. **Denial of Service (DoS)**:
    
    - **Definition**: An attack that aims to make a network service unavailable by overwhelming it with traffic.
    - **Distributed Denial of Service (DDoS)**: Multiple machines, often part of a botnet, are used to flood a target with traffic.
    - **Example**: A **Ping of Death** sends malformed packets to crash a system, or **SYN Flooding**, where numerous SYN requests overwhelm a server’s ability to respond.
    - **Prevention**: Rate limiting, firewalls, and intrusion detection/prevention systems (IDS/IPS).
4. **Secret Stealing**:
    
    - **Definition**: The theft of sensitive information such as passwords, encryption keys, or confidential data.
    - **Example**: The **Heartbleed vulnerability** in OpenSSL allowed attackers to read memory from a server, potentially exposing sensitive information.
    - **Prevention**: Regular patching, use of secure communication protocols (SSL/TLS).

---

### Key Terms

- **Encapsulation**: The process of adding headers (and sometimes footers) to data as it passes through the layers of the network stack. Each layer wraps data in its own header, and at the receiving end, the headers are removed layer by layer.
    
- **End-to-End Connectivity**: A principle of networking where communication is handled between the endpoints (hosts or applications), with intermediate devices (routers, switches) facilitating data transfer without altering it.
    
- **Firewalls**: Network security devices or software that control incoming and outgoing network traffic based on predetermined security rules. They act as a barrier between a trusted internal network and an untrusted external network.
    
- **Virtual Private Network (VPN)**: A service that encrypts internet traffic and routes it through a secure tunnel, hiding the user's IP address and ensuring privacy.
    
- **SSL/TLS (Secure Socket Layer/Transport Layer Security)**: Protocols for securing communication between web browsers and servers. SSL is the predecessor to TLS, which is now the standard for secure communication.
    
- **Authentication**: The process of verifying the identity of a user or device. Common authentication methods include passwords, biometrics, and cryptographic tokens.
    
- **Encryption**: The process of converting data into a coded format to prevent unauthorized access. Data is decrypted using a key or password.
    

---

## Reflection on the Layering Model

- The **IP hourglass model** reflects the simplicity of the Internet's design, with **IP (Internet Protocol)** as the narrow waist, supporting many different protocols at the lower (e.g., Ethernet) and upper layers (e.g., HTTP, DNS).
    
- **HTTP(S)** is becoming increasingly important as many modern applications (e.g., web services, REST APIs) run over it.
    

---

## Robustness Principle

- **"Be liberal in what you accept, and conservative in what you send."** — John Postel, RFC 761.
    
- This principle shaped the development of many Internet protocols, emphasizing resilience against incorrect or malicious input.