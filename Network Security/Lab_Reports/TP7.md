
---

# Network Security TP7

## **Introduction**

- Network packets are structured into layers: Application, Transport, Network, Link, Physical.
- Objective: Routing and filtering packets in Network and Transport layers.

---

## **Environment**

- Use two Linux VMs in VirtualBox with bridged connections:
    - **VM1**: Router
    - **VM2**: Client
- Commands:
    - `sudo su -` for root.
    - `hostname router` and `hostname client` to identify VMs.

---

## **Part I: Simple Routing**

1. **Disable ICMP Redirects**:
    
    ```bash
    echo 0 | sudo tee /proc/sys/net/ipv4/conf/*/send_redirects
    ```
    
2. **Routing Table**:
    - VM1: `ip a` and `ip route`
    - VM2: `ip a` and `ip route`
3. **Set Default Route on VM2**:
    
    ```bash
    ip route add default via <VM1_IP>
    ```
if we ping a domain name the DNS doesn't work so we have to ping 8.8.8.8 (since the vm doesn't forward the packets)
4. **Troubleshoot Routing**:
    - Enable forwarding:
        
        ```bash
        sysctl net.ipv4.ip_forward=1
        ```
        

---

## **Part II: Simple Firewalling**

1. **Block Ping to 8.8.8.8**:
    
    ```bash
    iptables -A OUTPUT --dst 8.8.8.8 -j DROP
    ```
    
2. **Filter Client Traffic**:
`iptables -A FORWARD --dst 8.8.8.8 -j DROP`


---

## **Part III: NAT (1/2)**

1. **Enable NAT on VM1**:
    
    ```bash
    iptables -t nat -A POSTROUTING -j MASQUERADE
    ```
    
2. **Verify NAT**:
    - Run `curl -L api.ipify.org` on VM2 before/after NAT.
this changes the "ip address to the router's one"
---

## **Part IV: NAT (2/2)**

1. **Redirect HTTP to VM2**:
    
    ```bash
    iptables -t nat -A PREROUTING --dst <VM1_IP> -p tcp --dport 80 -j DNAT --to-destination <VM2_IP>:80
    ```
    
2. **Traceroute**:
    - Add TTL increment rule:
        
        ```bash
        iptables -t mangle -A PREROUTING -i eth0 -j TTL --ttl-inc 1
        ```
When we increment the ttl

---

### **Part V: Firewalling 1/2**

11. **Difference between `-j DROP` and `-j REJECT`?**
    
    - `DROP`: Silently discards packets.
    - `REJECT`: Sends an error message back to the sender.
12. **Discuss observations with traffic.**
    
    - REJECT provides immediate feedback, whereas DROP gives no response.
13. **What does `iptables-save` do?**
    
    - Saves current rules into a file (`iptables.txt`).
14. **What does `iptables-restore` do?**
    
    - Restores rules from the saved file in one atomic action.

---
### **Part V: Firewalling 2/2**

15. **What does the script do?**
    
    - Automates rule management for starting, stopping, and reloading the firewall.
16. **Add rules for loopback:**
    
    ```bash
    iptables -A INPUT -i lo -j ACCEPT
    iptables -A OUTPUT -o lo -j ACCEPT
    ```
    
17. **Allow traffic from `people.irisa.fr`:**
    
    ```bash
    iptables -A INPUT --src people.irisa.fr -j ACCEPT
    ```
    
18. **Allow established or related connections:**
    
    ```bash
    iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
    ```
    
19. **Create `LOGDROP` chain:**
    
    ```bash
    iptables -N LOGDROP
    ```
    
20. **Add logging and drop rules to `LOGDROP`:**
    
    ```bash
    iptables -A LOGDROP -j LOG --log-prefix '[LOGDROP] '
    iptables -A LOGDROP -j DROP
    ```
    
21. **Redirect traffic to `LOGDROP`:**
    
    ```bash
    iptables -A INPUT -d www.facebook.com -j LOGDROP
    ```
    
22. **What do you observe in logs?**
    
    - Logs record dropped packets, visible with `dmesg` or syslog.
23. **DROP all in INPUT:**
    
    ```bash
    iptables -A INPUT -j DROP
    ```
    
24. **Can you access the internet after blocking INPUT?**
    
    - No, blocking INPUT prevents responses unless explicitly allowed.
25. **Allow traffic to port 80:**
    
    ```bash
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    ```
    
26. **Display rules with `nftables`:**
    
    ```bash
    nft list ruleset
    ```
    
    - Compare rule readability with `iptables`.
27. **Attach and comment script.**
    
    - Script functionality and improvements should be discussed.

---
