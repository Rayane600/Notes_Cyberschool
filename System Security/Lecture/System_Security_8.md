
---

## Access Control in Unix (Part 2)

### Access Control Lists (ACLs)
- **Traditional POSIX Model**: Defines permissions for three classes - owner, group, others.
  - Permissions: `read (r)`, `write (w)`, `execute (x)`.
  - `ls -l` displays these permissions (e.g., `-rw-r-- ---`).

- **ACL Entries**:
  - **Types**: Owner, Named User, Owning Group, Named Group, Mask, Others.
  - **Mask**: Limits permissions for named users/groups in ACL.
  - **Minimal ACL**: 3 entries (owner, group, others).
  - **Extended ACL**: More entries for fine-grained access control.

- **Unix ACL Commands**:
  - `getfacl`: Check ACLs.
  - `setfacl -m`: Modify/add ACLs for users or groups.

![[Pasted image 20241119144530.png]]

---

### User Identifiers (UIDs) and Process Ownership
- **UID Types**:
  - **Real UID (ruid)**: User who started the process.
  - **Effective UID (euid)**: Access rights for the process.
  - **Saved UID**: Stores the euid when a process is initiated.

- **Setting UIDs**:
  - System commands and calls (`setuid`, `seteuid`, etc.) manage UIDs, controlling process privileges and user switching.

---

### Discretionary Access Control (DAC)
- **DAC Model**: User-oriented; based on the identity of the requester.
  - Access rights are typically defined in an **Access Matrix**.
  - Matrix entries specify access rights per subject-object pairing.

- **DAC Implementation**:
  - **Access Control Lists (ACLs)**: Rights stored with objects, listing user/group access.
  - **Limitations**: Vulnerable to Trojan horse attacks, where malicious software uses legitimate privileges to bypass access restrictions.

- **Security Issues**:
  - **Trojan Horse Vulnerability**: Malicious entities can exploit DAC to gain unauthorized access.

---
