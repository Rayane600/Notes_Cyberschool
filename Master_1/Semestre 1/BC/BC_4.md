

### MAC and Authenticated Encryption:

### **Birthday Paradox**

- In a set of $D$ elements, selecting $\sqrt{D}$ elements at random results in a high probability of collision.
- Example: With $D = 365$, approximately 23 people are needed for a shared birthday.
- For sets $N$ and $M$ in a large set $D$, the expected number of collisions is $\frac{|N| \times |M|}{|D|}$.

---

### **Message Authentication Code (MAC)**

- **Purpose**: Ensures message integrity and authenticity.
- **Warning**: Encryption alone (e.g., CTR mode) does not guarantee integrity.

**Definition**:

1. **Key Generation**:
    - Input: Security parameter (e.g., 128 bits).
    - Output: Randomly distributed key $K$.
2. **Tag Generation**:
    - Input: Message $M \in {0,1}^*$ and key $K$.
    - Output: Tag $\tau \in {0,1}^t$ using $MAC_K(M)$.
3. **Verification**:
    - Input: $M$, $\tau$, and $K$.
    - Output: Accept or reject the tag.

---

### **Security**

- **Adversarial Goals**:
    
    1. **Key Recovery**: Extracting $K$.
    2. **Forgery**: Producing a valid MAC for any message (known or chosen by the adversary).
- **Adversarial Resources**:
    
    1. **Known Message Attack**: Access to pairs $(M, \tau)$ of valid MACs.
    2. **Chosen Message Attack (CMA)**: Access to the MAC generation algorithm.

**EUF-CMA** (Unforgeability Against Chosen Message Attacks):

- Measures the adversary's success in forging a valid tag under chosen message conditions.

**Generic Security**:

1. For a $t$-bit MAC, forgery probability is $\geq \frac{1}{2^t}$.
2. By the birthday paradox, $2^{t/2}$ MACs will likely contain collisions, exploitable for key recovery.

---

### **MAC vs. Digital Signatures**

- **Digital Signatures**:
    - Verifiable with a public key.
    - Guarantees non-repudiation.
    - Similar to handwritten signatures.
- **MACs**:
    - Relies on a shared secret key between users.
    - No public verification or non-repudiation.
    - High performance.

---

### **MAC Constructions**

1. **Example 1**:
    - For $F: {0,1}^k \times {0,1}^* \to {0,1}^t$ (random function):
        - Message $M = M_1, M_2, \ldots, M_m$.
        - Tag: $t = F_K(M_1) \oplus F_K(M_2) \oplus \ldots \oplus F_K(M_m)$.
2. **Example 2**:
    - For $F$ as above:
        - Compute $y_i = F_K(\langle i \rangle, M_i)$ for $i = 1, \ldots, m$.
        - Tag: $t = y_1 \oplus \ldots \oplus y_m$.

---

### **CBC-MAC**

- **Unencrypted CBC-MAC**:
    
    - $C_i = E_K(M_i \oplus C_{i-1})$, $MAC = C_m$.
    - Secure only for fixed-length messages.
- **Encrypted CBC-MAC (EMAC)**:
    
    - $MAC = E_{K'}(C_m)$.
    - Secure for arbitrary-length messages if fewer than $2^{n/2}$ MACs are computed.

---

### **Hash-Based MAC (HMAC)**

1. **Basic Scheme**:
    
    - $MAC_K(M) = H(K || M)$.
    - Vulnerable without proper padding and key management.
2. **HMAC**:
    
    - $HMAC_K(M) = H(K' \oplus opad, H(K' \oplus ipad, M))$, where:
        - $K'$: Derived key.
        - $ipad$ and $opad$: Fixed padding constants.

---

### **Encryption and Authentication Paradigms**

1. **MAC-Then-Encrypt (IPSec)**:
    
    - MAC is generated first, then encryption.
    - Integrity may not always be guaranteed.
2. **Encrypt-Then-MAC (SSL/TLS)**:
    
    - Secure if encryption and MAC are individually secure.
3. **MAC-And-Encrypt (SSH)**:
    
    - Non-secure mode.
    - MAC may leak information about plaintext.

---

### **Authenticated Encryption**

1. **AES-GCM**:
    
    - Combines AES in CTR mode for encryption with GMAC for authentication.
    - Based on Encrypt-Then-MAC paradigm.
2. **GMAC and Poly1305**:
    
    - GMAC: MAC for AES-GCM.
    - Poly1305: MAC for ChaCha-Poly1305.
    - Efficient evaluation using Horner’s scheme over finite fields:
        - AES-GCM: $F_{2^{128}}$.
        - Poly1305: $F_p$, $p = 2^{130} - 5$.
