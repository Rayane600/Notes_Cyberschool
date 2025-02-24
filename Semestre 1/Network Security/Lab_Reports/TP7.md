
---

# Network Security - TP7 Report

Jelidi--Daniel Rayane
Galmel Yann

---

## Part I: Simple Routing

### Q0: Disable ICMP Redirects

On VM1 (Router):

```bash
echo 0 | sudo tee /proc/sys/net/ipv4/conf/*/send_redirects
```

### Question 1: IP of VM1/Router and Its Routing Table

On VM1/Router:

```bash
ip a
ip route
```


- VM1 (Router) IP: `148.60.12.100` on `eth0`.
- Routing table on VM1  shows :
    
    ```
    default via 148.60.12.1 dev eth0
    148.60.12.0/24 dev eth0 proto kernel scope link src 148.60.12.100
    ```
    

The default route points to `148.60.12.1` , and the network `148.60.12.0/24` is directly reachable.

### Question 2: IP of VM2/Client and Its Routing Table

On VM2/Client:

```bash
ip a
ip route
```

- VM2 (Client) IP: `148.60.12.101` on `eth0`.
- Initial routing table on VM2:
    ```
    default via 148.60.12.1 dev eth0
    148.60.12.0/24 dev eth0 proto kernel scope link src 148.60.12.101
    ```
### Question 3: Change Default Route on VM2 to Use VM1 as a Router

Remove the current default route and add VM1 as the gateway:

```bash
ip route del default
ip route add default via 148.60.12.100
ip route show
```

**After change:**

```
default via 148.60.12.100 dev eth0
148.60.12.0/24 dev eth0 proto kernel scope link src 148.60.12.101
```

### Question 4: Can VM2 Access the Internet?

we tried:

```bash
ping 8.8.8.8
```

on VM2.

**Observation:**

- VM2 sends packets to VM1, verified with Wireshark on VM1.
- Initially, ping failed because VM1 by default does not forward packets.
- To fix this on VM1:
    
    ```bash
    sysctl -w net.ipv4.ip_forward=1
    ```
    

After enabling forwarding, VM2’s ping to `8.8.8.8` should now succeed. VM1 is now acting as a router.

---

## Part II: Simple Firewalling

### Question 5: Block Ping to 8.8.8.8 From VM1 Itself

On VM1:

```bash
iptables -A OUTPUT --dst 8.8.8.8 -j DROP
ping 8.8.8.8
```

**Answer:**  
The ping from VM1 will not receive any reply. The filtering works for VM1’s own traffic.

### Question 6: Block Traffic From VM2?

VM2’s pings still go through, because we only blocked in the OUTPUT chain on VM1, which affects VM1’s own traffic, not forwarded traffic.

To block forwarded traffic, we need a FORWARD rule.

### Question 7: Forward Rule to Block VM2’s Traffic to 8.8.8.8

On VM1:

```bash
iptables -A FORWARD --dst 8.8.8.8 -j DROP
```

Now VM2’s pings to 8.8.8.8 are blocked. After testing, we flushed the rules:

```bash
iptables -F
```

---

## Part III: NAT (1/2)

Before NAT, on VM2:

```bash
curl -L api.ipify.org
```

 We see `148.60.12.101` (VM2’s IP).

### Enable NAT (MASQUERADE) on VM1

On VM1:

```bash
iptables -t nat -A POSTROUTING -j MASQUERADE
```

On VM2 again:

```bash
curl -L api.ipify.org
```

**Answer/Explanation:**  
Now the seen IP is `148.60.12.100` (the router’s IP).

---

## Part IV: NAT (2/2)

### Redirect HTTP to VM2

On VM2 (Client):

```bash
apt update
apt install apache2
systemctl start apache2
```

Apache runs and listens on port 80.

On VM1 (Router):

```bash
iptables -t nat -A PREROUTING -d 148.60.12.100 -p tcp --dport 80 -j DNAT --to-destination 148.60.12.101:80
```

From the host machine, we visit  `http://148.60.12.100`. 

**Explanation:**  
The DNAT rule sends traffic destined for 148.60.12.100:80 to 148.60.12.101:80.

### Traceroute and TTL Increment

On VM2:

```bash
traceroute 8.8.8.8
```

Shows a normal route.

Then on VM1:

```bash
iptables -t mangle -A PREROUTING -i eth0 -j TTL --ttl-inc 1
```

 `traceroute` again on VM2. The hop count is affected because we are incrementing TTL, altering how traceroute perceives the route.

---

## Part V: Firewalling 1/2

Flush previous rules:

```bash
iptables -F
iptables -t nat -F
iptables -t mangle -F
```

1. Add DROP rule:
    
    ```bash
    iptables -A OUTPUT --dst 8.8.8.8 -j DROP
    ping 8.8.8.8
    ```
    
    The ping fails silently.
2. Replace DROP with REJECT:
    
    ```bash
    iptables -D OUTPUT --dst 8.8.8.8 -j DROP
    iptables -A OUTPUT --dst 8.8.8.8 -j REJECT
    ping 8.8.8.8
    ```
    
    Now we get an immediate error response.

### Question 11: Difference Between DROP and REJECT

- DROP: No response, silent discard.
- REJECT: Sends an error (e.g., ICMP Port Unreachable), providing immediate feedback.

### Question 12: Observations

With REJECT, we see immediate error; with DROP, the client times out.

### Question 13: `iptables-save`

```bash
iptables-save > iptables.txt
```

This saves current iptables rules to `iptables.txt`.

### Question 14: `iptables-restore`

```bash
iptables-restore < iptables.txt
iptables -L -v -n
```

Restores rules from the file atomically.

---

## Part V: Firewalling 2/2

### Question 15: Script Function

The script sets up a firewall configuration with start/stop/restart/status commands.

### Question 16: Loopback Rules

```bash
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
```

### Question 17: Allow from `people.irisa.fr`

We resolved it to `131.254.254.107` and added:

```bash
iptables -A INPUT -s 131.254.254.107 -j ACCEPT
```

### Question 18: Established/Related Connections



```bash
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

### Question 19 & 20: LOGDROP Chain

```bash
iptables -N LOGDROP
iptables -A LOGDROP -j LOG --log-prefix '[LOGDROP] '
iptables -A LOGDROP -j DROP
```

### Question 21: Redirect Facebook Traffic to LOGDROP

For `www.facebook.com` (157.240.202.35):

```bash
iptables -A INPUT -d 157.240.202.35 -j LOGDROP
```

### Question 22: Logs

When attempting to reach Facebook, `dmesg -w` or `tail -f /var/log/syslog` shows `[LOGDROP]` entries.

### Question 23: DROP Everything Else in INPUT


```bash
iptables -A INPUT -j DROP
```

### Question 24: Internet Access

Because we allowed `ESTABLISHED,RELATED` and certain safe rules, outgoing initiated connections should still work. New inbound connections not allowed by these rules will be dropped. Thus, regular web browsing (not to Facebook :-) ) should work since responses to initiated traffic are allowed.

### Question 25: Allowing Port 80

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### Question 26: Display Rules With nft

```bash
apt install nftables
nft list ruleset
```



### Question 27: Script Attachment and Comments

```bash 
#!/bin/bash

start (){
    # Safeguard rules
    iptables -A INPUT -i lo -j ACCEPT
    iptables -A OUTPUT -o lo -j ACCEPT
    
    # people.irisa.fr -> 131.254.254.107
    iptables -A INPUT -s 131.254.254.107 -j ACCEPT

    # Established or related connections
    iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

    # Block 8.8.8.8 on OUTPUT as per initial example
    iptables -A OUTPUT --dst 8.8.8.8 -j DROP

    # Create and setup LOGDROP chain
    iptables -N LOGDROP
    iptables -A LOGDROP -j LOG --log-prefix '[LOGDROP] '
    iptables -A LOGDROP -j DROP

    # www.facebook.com -> 157.240.202.35
    iptables -A INPUT -d 157.240.202.35 -j LOGDROP

    # Allow inbound HTTP
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT

    # Finally, drop all other INPUT
    iptables -A INPUT -j DROP
}

stop (){
    iptables -F
    iptables -X
    iptables -t nat -F
    iptables -t mangle -F
}

case $1 in
"start")
    start
    ;;
"stop")
    stop
    ;;
"restart"|"reload")
    stop
    start
    ;;
"status")
    iptables -L -v -n
    ;;
esac

```