
# Linux Security Projects

Hi! I'm **Salman Qureshi**, This repository showcases my hands-on **Linux security projects**, ranging from **enterprise-grade identity management and security hardening** to **focused experiments on intrusion detection, auditing, secure communications, and automation **. Each project demonstrates **practical implementation of Linux security concepts and tools**, suitable for real-world scenarios.

## 📂 Projects Overview

### 🔹 1. IdM Security Foundations
**Full-scale FreeIPA deployment on GCP for centralized identity and access management.**

**Skills & Technologies:** FreeIPA, Kerberos, HBAC, IAM, SUDO policies, GCP VPC/Subnets/Firewall, SSH & HTTPS management, Cloud IaC automation.

[View Project →](idm-security-foundations/README.md)

---

### 🔹 2. Linux Security Hardening Playbook
**Automated Linux security hardening using Ansible and industry-standard frameworks.**

**Highlights:**
* Intrusion Detection: AIDE for file integrity monitoring
* System Auditing: Auditd for real-time logging
* Access Control: SELinux mandatory access policies
* Encryption: TLS certificates for secure communications
* Compliance: OpenSCAP for CIS benchmark scanning and remediation
* Automation: Role-based Ansible playbooks for repeatable deployment

[View Project →](linux-security-hardening-playbook/README.md)

---


### 🔹 3. Individual Security Experiments
These smaller projects demonstrate **focused Linux security skills**:


| Project | Focus | Link |
|---------|-------|------|
| AIDE Intrusion Detection | File integrity monitoring | [README](individual-projects/AIDE-intrusion-detection/README.md) |
| Auditd System Logging | Centralized system event logging | [README](individual-projects/Auditd-system-logging/README.md) |
| SELinux Access Control | Mandatory access control policies | [README](individual-projects/SELinux-access-control/README.md) |
| TLS Secure Communication | Secure server-client communication | [README](individual-projects/TLS-secure-communication/README.md) |

---

## ⚙️ Key Skills Demonstrated

* **Linux Security:** File integrity, auditing, SELinux, TLS, CIS compliance
* **Cloud & Infrastructure:** GCP VM setup, VPC/Subnet/Firewall, multi-server IdM
* **Automation & IaC:** Ansible playbooks, CLI provisioning, repeatable deployment
* **Access & Identity Management:** Users, groups, HBAC, Kerberos, SUDO policies
* **Monitoring & Reporting:** Verification screenshots, logs, and compliance reports

## 📂 Repository Structure

Here's a **high-level overview** of this repo:
```
   linux-security-projects/
   ├── README.md
   ├── idm-security-foundations/
   │   ├── README.md
   │   ├── IdM_Security_Foundations-setup-doc.pdf
   │   └── screenshots/
   ├── individual-projects/
   │   ├── AIDE-intrusion-detection/
   │   ├── Auditd-system-logging/
   │   ├── SELinux-access-control/
   │   └── TLS-secure-communication/
   └── linux-security-hardening-playbook/
       ├── README.md
       ├── playbook.yml
       ├── reports/
       └── roles/
```

For more details, each project folder contains its **own README** with setup instructions, screenshots, and verification steps.

## 🚀 How to Explore

1. Click on any project link above to **view detailed setup instructions**.
2. Screenshots and reports are provided for verification of security implementations.
3. For automation-based projects (like the Hardening Playbook), follow the **Ansible commands and tags** in the project README.

---
---
