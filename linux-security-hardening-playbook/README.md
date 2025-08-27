# Linux Security Hardening Project 🛡️

Hi there! 👋 I'm Salman Qureshi, and this is my Linux Security Hardening project, where I explored and implemented multiple security practices on Linux systems using Ansible automation and individual security tools.

## Project Overview

This project demonstrates production-ready Linux security hardening through automation and industry-standard security frameworks. It implements multiple layers of security controls including intrusion detection, system auditing, access controls, encrypted communications, and CIS benchmark compliance.

The goal of this project is to demonstrate practical Linux security hardening by using a combination of tools and an automated Ansible playbook.

## What’s included?

* **Infrastructure as Code**: Ansible playbooks with role-based architecture
* **Compliance Framework**: CIS (Center for Internet Security) benchmarks
* **Security Monitoring**: Real-time file integrity and system auditing
* **Access Control**: Mandatory Access Control (MAC) with SELinux
* **Encryption**: TLS certificate management and secure communications

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

```

linux-security-hardening-playbook
    |-- playbook.yml
    |-- reports
    |   |-- aide
    |   |   `-- aide-report.txt
    |   |-- auditd
    |   |   `-- auditd-rules.txt
    |   |-- firewall
    |   |   `-- firewall-report.txt
    |   |-- openscap
    |   |   |-- cis-remediate.sh
    |   |   |-- cis-report-before.html
    |   |   |-- cis-report.html
    |   |   |-- cis-results-before.xml
    |   |   `-- cis-results.xml
    |   |-- selinux
    |   |   `-- selinux-status.txt
    |   `-- tls
    |       `-- selfsigned.crt
    `-- roles
        |-- aide
        |   `-- tasks
        |       `-- main.yml
        |-- auditd
        |   |-- handlers
        |   |   `-- main.yml
        |   `-- tasks
        |       `-- main.yml
        |-- firewall
        |   `-- tasks
        |       `-- main.yml
        |-- openscap
        |   `-- tasks
        |       `-- main.yml
        |-- selinux
        |   `-- tasks
        |       `-- main.yml
        `-- tls
            `-- tasks
                `-- main.yml

27 directories, 30 files

```

## How I Ran the Project

I first created the Ansible playbook (playbook.yml) along with role directories, each containing their main YAML files under `roles/<module>/tasks/main.yml`, to automate the deployment and configuration of all the security modules.

After that, I ran each module individually using its specific tag to make sure everything was working correctly and verified the results in the reports directory. For example, I used the following command structure (placeholder used for each module tag):

`ansible-playbook playbook.yml --tags <module-tag>`

Once all modules were confirmed to be working, I ran the entire playbook at once to apply all security hardening tasks sequentially:

`ansible-playbook playbook.yml`

This automatically generated all reports in the reports directory.

## Key Highlights 

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
| OpenSCAP | Compliance & remediation | `ansible-playbook playbook.yml --tags openscap` |
| Ansible | Automation of all tools | `ansible-playbook playbook.yml` |

## Conclusion

This project demonstrates a hands-on approach to Linux security hardening using automation and industry-standard tools. By combining AIDE, Auditd, SELinux, TLS, and OpenSCAP with an Ansible playbook, I was able to secure, monitor, and audit a Linux system effectively while generating verifiable reports.
