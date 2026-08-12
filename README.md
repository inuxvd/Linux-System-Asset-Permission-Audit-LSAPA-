# Linux-System-Asset-Permission-Audit-LSAPA-
This project documents a full security audit and hardening process performed on a Linux virtual machine. The goal was to evaluate user accounts, groups, permissions, SSH configuration, sudo privileges, and critical system directories, applying real-world security controls and validating system integrity after changes.


System Information
Commands used:
uname -a
lsb_release -a

## System Information
**Kernel Version:**  6.17.0-19-generic
**Distribution:** Ubuntu 24.04.4 LTS
**Environment:** VirtualBox VM
**Date of Audit:** August 2026

## Tools Used
- Ubuntu Linux
- VirtualBox
- Bash terminal
- Git & GitHub



**User & Group Audit**

**/etc/passwd**
- Identified valid users
- Identified system/service accounts
- Verified interactive shells
- Checked for anomalies or misconfigurations

**/etc/shadow**
- Verified password hashes
- Confirmed no empty passwords
- Checked password aging fields

**/etc/group**
- Reviewed group memberships
- Verified sudo/admin group assignments
- Checked for privilege escalation risks

 **Sudoers Audit**
Findings
Default rules found:
root ALL=(ALL:ALL) ALL
%sudo ALL=(ALL:ALL) ALL
%admin ALL=(ALL) ALL

Hardening Applied:
Defaults timestamp_timeout=5

Validation
File edited using visudo (safe method)

Syntax validated automatically

Sudo functionality confirmed after change


**SSH Audit
Initial Configuration**

Document found:

PasswordAuthentication yes

- MaxAuthTries 6
- LoginGraceTime 2m
- MaxSessions 10
- PubkeyAuthentication yes


Hardening Applied:

- PasswordAuthentication no
- MaxAuthTries 3
- LoginGraceTime 30s
- MaxSessions 2

**Validation**
- SSH restarted successfully
- Service status: active (running)
- Host key accepted
- Local SSH test performed
- “Broken pipe” behavior documented (expected for localhost)


**Directory Permissions Audit** 

/home

- Correct ownership
- No world-writable directories
- No cross-user access

/var/log
Owned by root

- Group adm
- No world-writable log files
- Log integrity preserved

/etc
- Owned by root
- Standard permissions (rw-r--r--)
- No misconfigurations
- No privilege escalation vectors

**Controls applied:**

- SSH hardening
- Sudoers hardening
- User/group audit
- Directory permission audit
- Log integrity verification
- System stability validation



During this project I learned how to perform a complete Linux security audit in a structured and professional way. I understood how users, groups, permissions, sudoers, SSH configuration, and critical directories all connect to the overall security posture of a system. I also learned how to apply real hardening controls without breaking the machine, and how to validate everything step-by-step.

One thing that surprised me was how simple some parts actually were once I saw the logic behind them. A lot of the commands were fast checks — confirm the output, make a judgment, and move on. I expected the process to be more complicated, but it was mostly about knowing where to look and why it matters. I also didn’t expect SSH to behave the way it did with the “broken pipe” message, but now I understand why that happens when connecting to localhost.

The most challenging part wasn’t the commands themselves — it was keeping track of how everything fits together: users, permissions, SSH settings, sudo rules, and directory ownership. I also realized that I don’t have every command memorized yet, but I can follow the logic and repeat the workflow quickly. The challenge was more about confidence and understanding the reasoning behind each step, not the technical difficulty.

Overall, this project helped me build real-world security intuition, not just command memorization.


## Remote SSH Access Setup (Windows Client → Ubuntu VM)

This update documents the successful configuration of secure remote access from a Windows 11 host (HP Envy) to an Ubuntu Server virtual machine running on a Lenovo laptop via VirtualBox.

### What Was Completed

- Switched VirtualBox networking from **NAT** to **Bridged Adapter** to obtain a valid LAN IP.
- Verified network connectivity and retrieved the VM’s IP using `ip a`.
- Enabled and started the SSH service on Ubuntu using:
  - `sudo systemctl enable --now ssh`
  - `sudo systemctl status ssh`
- Hardened SSH configuration by editing `/etc/ssh/sshd_config`:
  - `PasswordAuthentication no`
  - `PubkeyAuthentication yes`
- Generated a new ED25519 SSH key pair on the Windows client using:
  - `ssh-keygen -t ed25519`
- Installed the client’s public key into the Ubuntu VM:
  - Created `~/.ssh` directory
  - Added key to `~/.ssh/authorized_keys`
  - Set secure permissions (`chmod 700 ~/.ssh` and `chmod 600 ~/.ssh/authorized_keys`)
- Successfully established a **passwordless SSH connection**:
  - `ssh username@<VM-IP>`

### Result

Remote login is now fully functional and secured with key-based authentication.  
This completes the foundational setup for future cybersecurity labs, audits, and automation projects.


## Linux Permissions & System Audit (Remote SSH Lab)

This update documents a security-focused audit performed on the Ubuntu Server VM accessed remotely via SSH from a Windows 11 client. The goal was to understand Linux permissions, identify insecure configurations, and harden the system.

### What Was Completed

- Reviewed file and directory permissions using:
  - `ls -la`
  - `whoami`
  - `groups`
- Audited the `.ssh` directory and confirmed secure key permissions:
  - `.ssh` → 700
  - `authorized_keys` → 600
- Identified and corrected insecure group-write permissions in the home directory:
  - `chmod 755 tutorialdata`
  - `chmod 644 tutorialdata.zip`
- Performed a system-wide scan for world-writable files:
  - `sudo find / -type f -perm 777`
  - Result: No insecure files found.
- Audited system users and groups via:
  - `cat /etc/passwd`
  - `cat /etc/group`
- Verified login-capable accounts:
  - Only `root` and the primary user (`inux`) have valid shells.

### Result

The Ubuntu VM is now properly hardened with secure permissions, validated user accounts, and no world-writable files. This completes a practical Linux security audit performed entirely through remote SSH access.



