
# Secure Communication Protocol Design

## 1. Authenticated Key Exchange Protocol

### 1.1 Message Sequence

1. **Alice → Bob**: `{A, Na, "Communication request"}_Kb`
2. **Bob → Alice**: `{Na, kab}_Ka`
3. **Alice → Bob**: `{m, Na'}_kab`
4. **Bob → Alice**: `{Na'}_kab`

### 1.2 Cryptographic Algorithms Used

- **Asymmetric Encryption**: RSA (implemented manually)
- **Symmetric Encryption**: AES for subsequent communication
- **Nonce Generation**: Randomly generated nonces (`Na`, `Na'`) for freshness

### 1.3 Key Generation, Exchange, and Authentication

- **Key Generation**:
    - Both Alice and Bob generate RSA key pairs (`Ka_private`, `Ka_public`, `Kb_private`, `Kb_public`).
    - They may use self-signed certificates created via OpenSSL to associate their identities with their public keys.
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

## 2. Symmetric Communication Channel

### 2.1 Algorithms Used and Reasoning

- **Encryption**: AES in CBC mode using the shared secret key `kab`.
- **Message Authentication**: HMAC with SHA-256 to ensure integrity and authenticity.
- **Nonce Usage**: Nonces (`Na'`, `Nb'`) prevent replay attacks and ensure message freshness.

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
