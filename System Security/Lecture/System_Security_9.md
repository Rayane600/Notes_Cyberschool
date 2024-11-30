### Capabilities and System Security

---

#### **Part I: Introduction**

##### **Bank Analogy**

- Two ways to control access to valuables:
    1. **Access Control List (ACL):**
        - Bank authenticates individuals and maintains a list.
        - Pros: Centralized management.
        - Cons: Delegation is restricted; revocation requires bank intervention.
    2. **Capabilities:**
        - Bank issues keys to authorized individuals.
        - Pros: Decentralized, supports delegation.
        - Cons: Difficult to revoke access if keys are copied.

##### **Capabilities vs. ACL in a Hostile Environment**

- **Capabilities:**
    - Users can choose not to carry sensitive keys.
    - Employees can destroy unnecessary keys to limit potential damage.
- **ACL:**
    - Authentication depends entirely on the bank, making it susceptible to abuse during compromised states.

---

#### **Part II: Capabilities in Theory**

##### **Definition**

- Introduced in 1966 by Dennis and Van Horn.
- A capability is a token or key that grants access to an object or entity.
- Examples:
    - A movie ticket grants access to a movie.
    - A file descriptor allows operations on a file.

##### **Capabilities Types**

1. **Processes:**
    - Capabilities are dynamically assigned and tied to process privileges.
2. **Programs:**
    - Capabilities are linked to program files and stored in the inode.
3. **Users:**
    - Stored in user-specific configurations and inherited by processes initiated by the user.

##### **Using Capabilities**

- **Explicit Use:** Users explicitly present capabilities (e.g., authentication tokens).
- **Implicit Use:** Systems automatically check and validate capabilities (e.g., Linux process capabilities).

---

#### **Part III: Capability Storage and Operations**

##### **Storage Approaches**

1. **Protected Storage (Kernel-controlled):**
    - Kernel ensures integrity; users cannot modify capabilities.
2. **Unprotected Storage:**
    - Suitable for distributed environments.
    - Requires cryptographic checks to prevent tampering.
3. **Hybrid Storage:**
    - Real capabilities stored in the kernel, with users accessing read-only pointers.

##### **Basic Operations**

- **Create:** Assign a capability to a user or process.
- **Delete:** Permanently remove a capability.
- **Enable/Disable:** Temporarily toggle capabilities.
- **Delegate:** Share a capability with another user or process.
- **Revoke:** Remove a previously delegated capability (challenging to implement selectively).

---

#### **Part IV: Capabilities in Practice**

##### **Linux Capabilities**

- Divides root privileges into fine-grained capabilities.
- Example: Reading root-only files without full root privileges.
- Capabilities sets:
    1. **Effective Set:** Currently active capabilities.
    2. **Permitted Set:** Maximum capabilities a process can enable.
    3. **Inherited Set:** Capabilities passed to child processes.
    4. **Bounding Set:** Upper limit for a process’s capabilities.
    5. **Ambient Set:** Capabilities retained across `execve` calls.

##### **Practical Use Cases**

- File descriptors behave as capabilities:
    - Created during file open.
    - Allow access independent of ACL changes.

---

#### **Part V: Capabilities vs. ACL**

##### **Key Differences**

1. **Representation:**
    - ACLs are user-based, requiring authentication.
    - Capabilities are object-based, independent of user identity.
2. **Granularity:**
    - Capabilities enforce the principle of least privilege more effectively.
3. **Inheritance:**
    - ACLs pass all privileges to child processes.
    - Capabilities allow selective inheritance.
4. **Ambient Authority:**
    - ACLs allow implicit authority use, risking confused deputy problems.
    - Capabilities require explicit inclusion in the effective set.

---

#### **Additional Insights**

1. **Security Advantages of Capabilities:**
    - Avoids ambient authority risks.
    - Enhances containment in distributed and containerized environments (e.g., Docker).
2. **Challenges with Capabilities:**
    - Complexity in implementation.
    - Limited adoption due to lack of ready-to-use configurations compared to ACL-based systems.

##### **Meta-Conclusion**

- Most systems use ACLs due to historical emphasis on user-level sharing and ease of configuration.
- Capabilities offer superior control at the process level, aligning with modern security paradigms.

---

#### **Practical Commands in Linux**

- **View Capabilities:** `cat /proc/self/status` or `getpcaps`.
- **Assign File Capabilities:** `setcap CAP_DAC_READ_SEARCH=+eip <file>`.

---