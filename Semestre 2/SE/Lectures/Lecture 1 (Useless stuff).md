# Software Security Lecture Notes

---

## **1. Historical Context: The Morris Worm (1988)**  
**Chronology**:  
- **Nov 2, 1988**: Robert Morris releases the worm from MIT.  
- **Nov 3, 1988**: 2,500+ systems infected, disrupting the early Internet.  
- **Nov 4, 1988**: Administrators at Usenix Conference analyze and patch systems.  
- **Later**: Morris arrested, fined $10,000, and sentenced to community service.  

**Exploited Vulnerabilities**:  
1. **Weak Passwords**: Guessed reused passwords across systems.  
2. **Sendmail Debug Mode**: Allowed remote command execution.  
3. **"One Very Neat Trick"**: Buffer overflow in `fingerd` to execute arbitrary code.  

**Impact**:  
- Led to the creation of the **first CERT (Computer Emergency Response Team)**.  
- Highlighted the need for systematic vulnerability management.  

---

## **2. Evolution of Cyber Threats**  
**Key Examples**:  

| **Threat**          | **Year** | **Impact**                                                             |
| ------------------- | -------- | ---------------------------------------------------------------------- |
| **Stuxnet**         | 2010     | Sabotaged Iranian nuclear centrifuges via ICS vulnerabilities.         |
| **WannaCry**        | 2017     | Ransomware exploiting EternalBlue (Windows SMB vulnerability).         |
| **NotPetya**        | 2017     | Masqueraded as ransomware but aimed to destroy data (Ukraine-focused). |
| **Pegasus Spyware** | 2016+    | Infected mobile devices to extract data (developed by NSO Group).      |

---

## **3. Software Security Fundamentals**  
**Goals**:  
- Prevent unauthorized access/modification of data.  
- Ensure program integrity and availability.  

**Types of Vulnerabilities**:  
4. **Remote/Local Exploits**: Attack from outside/inside the network.  
5. **Privilege Escalation**: Gain higher access (e.g., root access).  
6. **Arbitrary Code Execution**: Inject and run malicious code.  
7. **Denial of Service (DoS)**: Overload systems to disrupt service.  

**Common Causes**:  
- **Buffer Overflows**: Writing beyond allocated memory (e.g., `gets()` in C).  
- **Insecure Input Handling**: SQL injection, XSS.  
- **Misconfigured Permissions**: Weak passwords, open ports.  

---

## **4. Vulnerability Management**  
**CVE (Common Vulnerabilities and Exposures)**:  
- Unique IDs (e.g., `CVE-2014-0160` for Heartbleed) for tracking vulnerabilities.  
- Managed by **MITRE Corporation** and **CVE Numbering Authorities (CNAs)**.  

**CVSS (Common Vulnerability Scoring System)**:  
- Scores vulnerabilities (0-10) based on:  
  - **Exploitability** (e.g., network access required).  
  - **Impact** (e.g., data confidentiality loss).  

**Example CVSS Calculation**:  
```  
Exploitability = 8.22 × AV × AC × PR × UI  
Impact = 6.42 × [1 - (1 - C) × (1 - I) × (1 - A)]  
Base Score = Exploitability + Impact  
```  

---

## **5. Architectural Models & Exploits**  
**Harvard vs. Princeton Architecture**:  
- **Harvard**: Separates code and data memory (safer, used in programming).  
- **Princeton**: Shared memory (flexible but prone to code injection).  

**Example Exploit**:  
```c  
#include <stdio.h>  
int main() {  
    int i = 0;  
    foo();  // Modifies return address to skip "i = 1"  
    i = 1;  
    printf("%d\n", i);  // Output: 0  
}  
```  
**Takeaway**: Memory manipulation can alter program flow unexpectedly.  

---
