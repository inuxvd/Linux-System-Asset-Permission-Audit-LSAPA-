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





