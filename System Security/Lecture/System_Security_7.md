A refaire solo

---

## System Security Overview

### Access Control
- **Definition**: Allows only authorized users, programs, or processes to access resources.
- **Key Components**:
  - Identify and authenticate users.
  - Record and monitor access attempts.
  - Enforce rules for granting/denying access.

### Security Models
- **System Model**: High-level description of system functions.
- **Security Policy**: Defines system security requirements.
- **Security Model**: Combination of the system model and security policy.

### Policy vs. Mechanism
- **Access Control Policy Models**:
  - **DAC (Discretionary Access Control)**: User-defined access rights.
  - **MAC (Mandatory Access Control)**: System-enforced access rules.
- **Mechanisms**: Tools to implement policies (e.g., access control matrices, lists, capabilities).

### Reference Monitor
- Enforces access control through:
  - **Invocation**: Always enforces access control.
  - **Tamperproofing**: Cannot be bypassed or modified.
  - **Small Size**: Simplifies analysis and assurance.

### UNIX Access Control
- **Principals**: Users (UID) and groups (GID).
- **Superuser**: UID 0, broad privileges (typically “root”).
- **File Permissions**:
  - Read (`r`), Write (`w`), Execute (`x`), categorized for Owner, Group, and Others.
  - **Octal Notation**: Permissions represented by digits (e.g., `777`).

### Special File Permissions
- **SetUID/SetGID**: Execute files with owner’s UID/GID.
- **Sticky Bit**: Only file owners can delete files in shared directories.

---

### Key UNIX Commands

#### User & Group Management
- **useradd**: Create a user.
- **usermod**: Modify a user.
- **userdel**: Delete a user.
- **groupadd**: Create a group.
- **groupmod**: Modify a group.
- **groupdel**: Delete a group.

#### Permission Management
- **chmod**: Change file mode bits.
- **chown**: Change file owner.
- **chgrp**: Change file group.
- **umask**: Set default permissions for new files/directories.

---

## Principles of Least Privilege
- Assign minimum privileges necessary to reduce security risks.
- Limits the “attack surface” by restricting excess privileges.

---

### System Architecture
- **Monolithic vs. Component Design**:
  - Monolithic: Integrated, less modular.
  - Component: Separate, specialized components for improved security.
