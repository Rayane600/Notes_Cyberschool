
# Secure Communication Protocol Design

## 1. Authenticated Key Exchange Protocol
### 1.1 Certificate Generation and exchange:
- First to generate the certificates run the following commands 
```bash
pyhton3 client_setup.py
pyhton3 server_setup.py
```
this will generate the certificates for the client and the server which will contain the public keys of each signed with their private key using RSA 4096. 
- We chose RSA 4096 because it offered good security and acceptable generation time for prime numbers.
- The certificates are then exchanged and and verified by the remote party.
- If the certificates are confirmed to be valid the protocol can continue.
### 1.2 Mutual Authentication:
- Now That we have a valid certificate, to be sure that the remote party didn't just steal another certificate and sent it we are going to go through an authentication phase where the server and client will mutually send each other challenges and respond to one another to confirm their identities.
- This step is handled by the exchange of signed messages and the verification of signatures.
### 1.3 Message Sequence

1. **Alice → Bob**: `{A, Na, "Communication request"}_Kb`
2. **Bob → Alice**: `{Na, kab}_Ka`
3. **Alice → Bob**: `{m, Na'}_kab`
4. **Bob → Alice**: `{Na'}_kab`

### 1.4 Cryptographic Algorithms Used

- **Asymmetric Encryption**: RSA 4096 was used just as in the first step.
- **Symmetric Encryption**: AES for subsequent communication once the shared secret is determined. We chose to use AEC-CBC since it would assure the security of the information since each block cipher depends on the block before, the same plain text will not be ciphered to the same output in the message.
- **HMAC** : We used HMAC to guarantee message integrity. 
- **Nonce Generation**: Randomly generated nonces (`Na`, `Na'`) for freshness

### 1.3 Key Generation, Exchange, and Authentication

- **Key Generation**:
    - Both Alice and Bob generate RSA key pairs (`Ka_private`, `Ka_public`, `Kb_private`, `Kb_public`).
    - They  use self-signed certificates to associate their identities with their public keys.
- **Key Exchange and Authentication**:
    1. **Step 1**:
        - Alice encrypts her identity `A`, a nonce `Na`, and a message `"Communication request"` with Bob's public key `Kb_public`.
        - This ensures that only Bob can decrypt the message, providing confidentiality.
    2. **Step 2**:
        - Bob decrypts the message with his private key `Kb_private`.
        - Bob verifies the nonce `Na` to confirm the message's freshness.
        - Bob generates a symmetric key `kab` for AES encryption.
        - Bob encrypts `Na` and `kab` with Alice's public key `Ka_public` and sends it back.
        - By including `Na`, Bob proves he decrypted Alice's message, providing mutual authentication.
    3. **Step 3**:
        - Alice decrypts the message with her private key `Ka_private`.
        - Alice verifies `Na` to confirm the response is linked to her request.
        - She obtains `kab`, establishing a shared secret.
- **Mutual Authentication**:
    - **Alice authenticates Bob** because only Bob could have decrypted her initial message and responded with the correct `Na`.
    - **Bob authenticates Alice** because only Alice could decrypt his message encrypted with `Ka_public`.

### 1.4 Fallback Mechanisms and Error Handling

- If nonce verification fails at any step, the protocol is aborted.
- Timeouts are implemented to handle cases where expected messages are not received.
- If decryption fails due to incorrect keys, an error is logged, and the session is terminated.
- If certificates are not validated the session is terminated.

## 2. Symmetric Communication Channel

### 2.1 Algorithms Used and Reasoning

- **Encryption**: AES in CBC mode using the shared secret key `kab`.
- **Message Authentication**: HMAC with SHA-256 to ensure integrity and authenticity.
- **Nonce Usage**: Nonces (`Na'`, `Nb'`) prevent replay attacks and ensure message freshness. They are randomly generated 16 byte nonces.

### 2.2 Session Security and Maintenance

- **Secure Transmission**:
    - Messages are encrypted with AES using `kab`.
    - Each message includes a new nonce (`Na'`, `Nb'`) and an HMAC.
- **Session Termination**:
    - A special termination message encrypted and authenticated with `kab` signals the end of the session.
    - Both parties securely delete `kab` from memory upon session termination.

### 2.3 Measures Against Known Attacks

- **Replay Attacks**:
    - Nonces are used in each message.
- **Man-in-the-Middle Attacks**:
    - Mutual authentication using RSA keys prevents impersonation.
- **Message Integrity**:
    - HMAC ensures that any tampering with the message can be detected.

### 2.4 Limitations

- **Public Key Distribution**:
    - Without a trusted certificate authority, public keys must be exchanged securely beforehand to prevent man-in-the-middle attacks during key exchange.
- **No Perfect Forward Secrecy**:
    - Compromise of long-term keys (`Ka_private`, `Kb_private`) could jeopardize past communications.
- **Nonce Management**:
    - Requires careful handling to avoid nonce reuse, which could lead to vulnerabilities.

---
## 3. Testing results: 

### 3.1 Prime number generation:
- For the generation of the keys of size 4096 for RSA, we found that the generation was taking too much time for our liking (it was probably caused by our implementation of miller-Rabin) so we decided to try to add multiprocessing since we just needed to find 2 prime numbers no matter the order so having multiple threads running could boost performance, which it did, we went from about 15-20 seconds to about a stable 5 seconds which is acceptable to generate a certificate.
