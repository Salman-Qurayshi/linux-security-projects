Here is your README content in a single code block of Markdown:
MarkDown
# Linux Security Hardening Project 🛡️

Hi there! 👋 I'm Salman Qureshi, and this is my Linux Security Hardening project, where I explored and implemented multiple security practices on Linux systems using Ansible automation and individual security tools.

This project showcases hands-on work securing Linux environments, monitoring systems, and managing secure communications.

## Project Overview

The goal of this project is to demonstrate practical Linux security hardening by using a combination of tools and an automated Ansible playbook.

## What’s included?

* **AIDE (Advanced Intrusion Detection Environment)**: 
  * Detects unauthorized file changes.
  * Creates a database of file checksums and can alert when changes occur.
* **Auditd (Linux Auditing System)**: 
  * Tracks and logs system events.
  * Monitors commands, file accesses, and configuration changes.
* **SELinux (Security-Enhanced Linux)**: 
  * Enforces access control policies on processes and files.
  * Helps contain security breaches by restricting process permissions.
* **TLS (Transport Layer Security)**: 
  * Ensures encrypted network communication.
  * Generates self-signed certificates to secure HTTP communications.
* **OpenSCAP**: 
  * Scans and remediates systems according to CIS benchmarks.
  * Generates detailed compliance reports and optionally applies fixes automatically.
* **Ansible Playbook**: 
  * Automates the deployment and configuration of all tools.
  * Allows running tasks individually using tags or all at once.

## Project Structure

(Here you can paste the tree output from VSCode)

## How I Ran the Project

### 1️⃣ AIDE - Intrusion Detection

* Initialize AIDE Database: `ansible-playbook playbook.yml --tags aide`
* Verify reports: `reports/aide/aide-report.txt`

### 2️⃣ Auditd - System Logging

* Apply Audit Rules: `ansible-playbook playbook.yml --tags auditd`
* Check logs: `reports/auditd/auditd-rules.txt`

### 3️⃣ SELinux - Access Control

* Enable/Check SELinux: `ansible-playbook playbook.yml --tags selinux`
* Check status report: `reports/selinux/selinux-status.txt`

### 4️⃣ TLS - Secure Network Communication

* Generate Self-Signed Certificates: `ansible-playbook playbook.yml --tags tls`
* Check certificate: `reports/tls/selfsigned.crt`

### 5️⃣ OpenSCAP - Compliance Scanning

* Scan & Remediate with OpenSCAP: `ansible-playbook playbook.yml --tags openscap -K`
* Reports:
  + Before remediation: `reports/openscap/cis-report-before.html`
  + After remediation: `reports/openscap/cis-report.html`
  + Results XML: `reports/openscap/cis-results.xml`

### 6️⃣ Run All Tasks at Once

* `ansible-playbook playbook.yml -K`
* This runs AIDE, Auditd, SELinux, TLS, and OpenSCAP sequentially.
* All reports and changes are generated automatically.

## Key Highlights / Portfolio Notes

* Automated Linux security hardening using Ansible roles.
* Hands-on with intrusion detection, auditing, access control, TLS encryption, and compliance scanning.
* Generated professional reports for verification.
* Practiced step-by-step testing and verification for each security tool.
* Demonstrated ability to use automation and manual verification together.

## Tools & Commands Summary

| Tool | Purpose | Command Example |
| --- | --- | --- |
| AIDE | File integrity monitoring | `ansible-playbook playbook.yml --tags aide` |
| Auditd | System auditing & logging | `ansible-playbook playbook.yml --tags auditd` |
| SELinux | Access control policies | `ansible-playbook playbook.yml --tags selinux` |
| TLS | Encrypt network communication | `ansible-playbook playbook.yml --tags tls` |
| OpenSCAP | Compliance & remediation | `ansible-playbook playbook.yml --tags openscap -K` |
| Ansible | Automation of all tools | `ansible-playbook playbook.yml -K` |

## Next Steps / Ideas

* Add real CA certificates instead of self-signed.
* Extend OpenSCAP remediations for more CIS benchmarks.
* Integrate centralized logging and alerts for Auditd and AIDE.
* Document before/after system snapshots for each tool.

This project demonstrates practical Linux security hardening, combining automation, monitoring, and compliance—all skills valuable for cybersecurity and DevOps portfolios.
