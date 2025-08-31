___
# Network Security - TP9 Report

This lab was completed by two groups working together, so the operations are the same, and we are submitting a joint report.

Group 1 :
- Jelidi--Daniel Rayane
- Galmel Yann
Group 2 : 
- Michel Vincent 
- Verdes Florian 


___
#### **Part I: Network Setup**

1. **Q1: What is your IP address inside your local network?**
    
    - **Answer:** My IP address is `192.168.1.18/24` . This was determined using the `ip a` command.
2. **Q2: Add the `192.168.1.80/24` IP address to your computer, which command did you use?**
    
    - **Answer:** I used the following command:
        
        ```bash
        sudo ip addr add 192.168.1.80/24 dev wlan0
        ```
        
3. **Q3: What is the IP address of your router?**
    
    - **Answer:** My router's IP address is `192.168.1.1`. This was identified using the `ip route` command.
4. **Q4: What does the interface look like? How did you find the password?**
    
    - **Answer:** The interface is a web GUI accessible via `http://192.168.1.1`. The password was "password".
5. **Q5: Add a rule to redirect traffic on port 80/TCP.**
    
    - **Answer:** I added a NAT rule in the router's Firewall/NAT section to redirect port `80/TCP` from the public IP to `192.168.0.80/24:8080`.
6. **Q6: Launch the webserver and access it externally.**
    
    - **Answer:** I launched the webserver with the following command:
        
        ```bash
        python3  http.server
        ```
        
        Using my phone's cellular connection, I accessed `http://86.229.222.114` and verified that the webserver was serving content.
7. **Q7: Summarize what you did with a diagram.**
    
    - **Answer:**
        
        ```
        External Client ---> Public_IP:80 ---> Router (NAT) --       -> 192.168.1.80:8080 ---> Webserver
        ```
---

#### **Part II: Domain Name Setup and REST**

1. **Q8: Which DNS servers have the information for the zone?**
    
    - **Answer:** The DNS servers for `netsec2223.eu.org` are `ns1.dynv6.com`, `ns2.dynv6.com`, and `ns3.dynv6.com`.
    command used :
    `nslookup -type=ns netsec2223.eu.org`
1. **Q9: Recursive query for a subdomain?**
    `dig +trace netsec2223.eu.org`
3. **Q10: Find the zone ID.**
    
    - **Answer:** Used the following command:
```bash 
curl -H "Authorization:BearerXCqXvdqJMWY1GEyCdcBZZWnwm4F6Ub" https://dynv6.com/api/v2/zones
```
- "id":3582646
4. **Q11: Get the list of DNS records.**
```bash
curl -H "Authorization: Bearer XCqXvdqJMWY1GEyCdcBZZWnwm4F6Ub" https://dynv6.com/api/v2/zones/3582646/records
```
5. **Q12: Add a new DNS record.**
    - **Answer:** Added a new record for `mydomain.netsec2223.eu.org` using:
        ```bash
curl -X POST \                                                
 -H "Authorization: Bearer XCqXvdqJMWY1GEyCdcBZZWnwm4F6Ub" \  
 -H "Content-Type: application/json" \  
 -d '{"name":"uwu","type":"A","data":"86.229.222.114"}' \  
 https://dynv6.com/api/v2/zones/3582646/records
```
        
6. **Q13: Visit the domain.**
    
    - **Answer:** Accessed `http://uwu.netsec2223.eu.org` from my phone.
7. **Q14: Summarize with a diagram.**
    
    - **Answer:**
  External Client ---> DNS Query ---> dynv6 ---> publicIP ---> Webserver


---

#### **Part III: Apache and HTTPS**

1. **Q15: Install Apache.**
```bash
yay -S apache
```
2. **Q16: Redirect ports.**
    -  Updated NAT rules for ports `80` and `443`.
3. **Q18: Obtain SSL Certificate.**
    
    - **Answer:** Used:
        
```bash
sudo certbot --dry-run certonly --manual -d uwu.netsec2223.eu.org
```

4. **Q19: Role of the challenge.**
    
    - **Answer:** The challenge ensures ownership of the domain by placing a specific file for validation
5. **Q20: Issue a real certificate.**
    
    - **Answer:** Used:
   ```bash
        sudo certbot -d uwu.netsec2223.eu.org
        ```
- Using arch the setup was a bit trickier to do :-) .

6. **Q21: Checked for availability.**
- Connection to `https://uwu.netsec2223.eu.org` was successfull

7. **Q22: Summarize with a diagram**
External Client ---> HTTPS ---> Apache ---> Webserver Content
8. **Q23: SCT value**
We checked on the browser for the SCT 
the Log ID is: `CF:11:56:EE:D5:2E:7C:AF:F3:87:5B:D9:69:2E:9B:E9:1A:71:67:4A:B0:17:EC:AC:01:D2:5B:77:CE:CC:3B:08`
9. **Q24: crt.sh**
We checked our certificates on crt.sh (there was only pre-certificates).
10. **Q25: Drawbacks of certificate transparency**
There is an increase of activity after the disclosure of the certificate.
#### **Part IV: IPv6**

1. **Q26: Add IPv6 support.**
we added the line `Listen [::]:443` in `/etc/httpd/conf/httpd-ssl.conf`
Our server answers with Ipv6

---

#### **Part V: Cleanup and Conclusion**

- Removed installed packages:
    
    ```
    yay -R certbot certbot-apache apache
    ```
    
- Reverted network and NAT changes.