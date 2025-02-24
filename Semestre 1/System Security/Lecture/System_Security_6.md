
# System Security Course Notes

## Part I: Introduction

### Identification, Authentication, Authorization (IAA)
- **Identification**: Who the users are.
- **Authentication**: How users prove their identity.
- **Authorization**: What users are allowed to do.

### Triple A Security Model
1. **Identify**: Maps a real person to a virtual account.
2. **Authenticate**: Prove that the account belongs to the user.
3. **Authorize**: Verify what resources the user can access.
4. **Accounting**: Logs and monitors user activity.

### Identification Example (Linux)
- The kernel knows numeric user IDs.
- Username mapping: Parse `/etc/passwd` or use `getpwuid()` with the `geteuid()` system call.

### Authorization Example (Linux)
- Enforces Discretionary Access Control (DAC) and Mandatory Access Control (MAC).
- `setuid()` system call changes user identity to run programs with different privileges.

## Part II: Authentication

### Authentication Process
- **Sign-up**: Slow, precise process to establish identity.
- **Sign-in**: Quick verification linking a user to an account.

### Factors of Authentication
1. **Knowledge Factor**: Something you know (e.g., password).
2. **Possession Factor**: Something you have (e.g., phone number, cryptographic token).
3. **Inherence Factor**: Something you are (e.g., fingerprint, facial recognition).

### Biometric Authentication
- Not 100% accurate due to measurement changes and possible attacks (e.g., fake fingerprints).
- Easy to collect but approximate matching is used.

## Part III: Passwords and Attacks

### Why Passwords?
- Ubiquitous in authentication.
- Can derive encryption keys.
- Low entropy, making them vulnerable to brute-force attacks.

### Authentication System Components
1. **A**: Identity information (password).
2. **C**: Stored authentication data.
3. **F**: Function mapping A → C.
4. **L**: Logic to verify identity.

### Hash Functions
- **One-way**: Hard to deduce input from output.
- **Collision resistance**: Hard to find two inputs producing the same output.
- Used to protect passwords during storage.

### Password Attacks
- **Online attack**: Try guesses during login attempts.
- **Offline attack**: Crack password hashes from stolen data using tools like John the Ripper.

### Preventing Attacks
- **Rate limiting**: Make login attempts slower to prevent brute-force.
- **Salting**: Add randomness to password hashes to slow dictionary attacks.
- **Password Aging**: Force periodic password changes.

### Example of Password Length Calculation
- A password of 7 characters in a 62-character alphanumeric set provides reasonable security over a year-long period with a guessing rate of 10,000 per second.

## Part IV: Unix Password Storage

### /etc/passwd vs /etc/shadow
- **/etc/passwd**: World-readable file containing user IDs.
- **/etc/shadow**: Contains hashed passwords, readable only by root.

### /etc/shadow File Format
- Username.
- Encrypted password using `crypt(3)`.
- Password aging info (e.g., last changed, expiration warnings).

### Hash Functions Used in Linux
- **yescrypt** (recommended).
- **sha256crypt** (weak).
- **md5crypt** (too weak).
- **descrypt** (limited to 8-character passwords).

## Part V: Pluggable Authentication Modules (PAM)

### Overview
- PAM allows centralized authentication, separating authentication logic from applications.
- Modules are added to support different authentication methods (e.g., fingerprint).

### PAM Configuration
- Configuration files are located in `/etc/pam.d/`.
- Types of rules:
  - **auth**: User authentication.
  - **account**: Verifies if user is allowed to log in.
  - **session**: Manages user sessions (e.g., mount home directory).
  - **password**: Updates user credentials.

### PAM Control Flags
- **requisite**: Fail immediately.
- **required**: Fail but continue checking other rules.
- **sufficient**: Succeed and stop checking further rules.
- **optional**: Result is ignored.

### Example PAM Module Usage
- `pam_unix`: Standard Unix authentication using `/etc/shadow`.
- `pam_pwquality`: Enforces password strength rules.

