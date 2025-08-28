# Linux Identity & Security Foundations Project 🛡️

Hi! I'm **Salman Qureshi**, and this project demonstrates the implementation of a **robust Identity Management (IdM) infrastructure** using **FreeIPA** on **Google Cloud Platform (GCP)**. The deployment showcases **enterprise-grade identity and access management capabilities**, including centralized authentication, authorization, and security policy enforcement across multiple servers.

## 📝 Project Overview

This project focuses on **designing a secure, scalable Linux identity and access management environment** with automation, following best practices in cloud security and Linux administration:

* **Identity & Access Management (IAM / IdM):** Centralized authentication and authorization using **FreeIPA**, enforcing consistent user and group policies across all hosts.
* **Authentication & Security:** Integration of **Kerberos** for secure ticket-based authentication and **PAM** for system login management.
* **Access Control:** Implementation of **Host-Based Access Control (HBAC) rules** to restrict access based on user, host, and service.
* **Infrastructure as Code (IaC):** Automated provisioning of GCP network and compute resources via **gcloud CLI**, ensuring repeatable and consistent deployments.
* **Security Best Practices:** Secure host configuration using **firewalld**, **SSH key authentication**, **FQDN-based hostnames**, and private IP addressing.

This project demonstrates **a full-stack approach to Linux security, identity management, and cloud infrastructure automation** — showcasing skills applicable to real-world enterprise environments.

## 🛠️ Skills Demonstrated

* **Cloud Infrastructure:** Architected and deployed a multi-server environment using **GCP VPCs, subnets, and firewall rules**.
* **Identity & Access Management:** Implemented **centralized authentication**, user/group management, HBAC rules, and SUDO policies using FreeIPA.
* **Networking:** Configured hosts with **FQDNs**, managed `/etc/hosts` entries, and used **SSH tunneling** to securely access the FreeIPA web interface.
* **Linux System Administration:** Performed **system updates, package management (dnf)**, and service management via `systemctl`.
* **Security Configuration:** Applied **firewalld rules** and SSH key-based authentication to enforce a secure baseline.
* **Command Line Automation:** Leveraged **gcloud, ipa-server-install, and ipa-client-install** commands to follow **Infrastructure as Code principles**.
* **Web-Based Management:** Demonstrated **secure HTTPS-based administration** of the IdM infrastructure via SSH tunnels.
* **Monitoring & Validation:** Validated host enrollment, user authentication, service availability, and access control enforcement.

## 🌐 Project Architecture

The project was deployed on **Google Cloud Platform** with the following custom infrastructure:

* **VPC:** `custom-vpc` — isolated network for security.
* **Subnet:** `custom-subnet` (`10.20.0.0/24`) in `me-central1` (Middle East - Qatar).
* **Firewall Rules:** `allow-ssh-http-https` — allows TCP 22, 80, 443 from anywhere (`0.0.0.0/0`).
* **VMs:**
  * `idm-primary` — FreeIPA server (`e2-standard-4`, 50 GB disk)
  * `idm-replica1` — FreeIPA replica (`e2-standard-2`, 30 GB disk)
  * `idm-replica2` — FreeIPA replica (`e2-standard-2`, 30 GB disk)

The architecture ensures **segregated network layers, secure host access, and scalable identity management**.


## Project Implementation & Configuration

This section documents the deployment and configuration steps for the Linux Identity & Security Foundations project. All commands and configurations reflect real-world enterprise practices, emphasizing security, automation, and repeatability.

### 1️⃣ Cloud Infrastructure Provisioning (IaC with CLI)

#### Custom VPC & Subnet Creation

   # Create a custom VPC
   gcloud compute networks create custom-vpc --subnet-mode=custom

   # Create a subnet in the Middle East (Qatar) region
   gcloud compute networks subnets create custom-subnet \
     --network=custom-vpc \
     --region=me-central1 \
     --range=10.20.0.0/24

#### Firewall Rules – Allow SSH, HTTP, HTTPS

   gcloud compute firewall-rules create allow-ssh-http-https \
     --network=custom-vpc \
     --allow tcp:22,tcp:80,tcp:443 \
     --source-ranges=0.0.0.0/0 \
     --description="Allow SSH, HTTP, HTTPS from anywhere"

#### VM Provisioning – Primary and Replica FreeIPA Servers

   # Primary FreeIPA server
   gcloud compute instances create idm-primary \
     --zone=me-central1-a \
     --machine-type=e2-standard-4 \
     --subnet=custom-subnet \
     --private-network-ip=10.20.0.10 \
     --image=centos-stream-9-v20250812 \
     --image-project=centos-cloud \
     --boot-disk-size=50GB \
     --tags=idm-server \
     --metadata=enable-oslogin=FALSE

   # Replica 1
   gcloud compute instances create idm-replica1 \
     --zone=me-central1-a \
     --machine-type=e2-standard-2 \
     --subnet=custom-subnet \
     --private-network-ip=10.20.0.11 \
     --image=centos-stream-9-v20250812 \
     --image-project=centos-cloud \
     --boot-disk-size=30GB \
     --tags=idm-server \
     --metadata=enable-oslogin=FALSE

   # Replica 2
   gcloud compute instances create idm-replica2 \
     --zone=me-central1-a \
     --machine-type=e2-standard-2 \
     --subnet=custom-subnet \
     --private-network-ip=10.20.0.12 \
     --image=centos-stream-9-v20250812 \
     --image-project=centos-cloud \
     --boot-disk-size=30GB \
     --tags=idm-server \
     --metadata=enable-oslogin=FALSE

### 2️⃣ Base Server Configuration

Performed on all nodes (Primary & Replicas):

#### 🔹 System Update & Package Installation

   sudo dnf update -y
   sudo dnf install -y vim bash-completion firewalld

#### 🔹 Hostname Configuration

   # Example for idm-primary
   sudo hostnamectl set-hostname idm-primary.lab.local

   # Replicas
   sudo hostnamectl set-hostname idm-replica1.lab.local
   sudo hostnamectl set-hostname idm-replica2.lab.local

#### 🔹 /etc/hosts Configuration

Configured FQDNs and kept GCP internal hostnames as aliases:

   10.20.0.10   idm-primary.lab.local   idm-primary
   10.20.0.11   idm-replica1.lab.local  idm-replica1
   10.20.0.12   idm-replica2.lab.local  idm-replica2

#### 🔹 Firewalld Configuration

   sudo systemctl enable --now firewalld
   sudo firewall-cmd --permanent --add-service=ssh
   sudo firewall-cmd --reload

This ensures baseline host-level security in addition to GCP firewall rules.

### 3️⃣ FreeIPA Server Installation (idm-primary)

#### 🔹 Install Server Packages

   sudo dnf install -y ipa-server ipa-server-dns bind-dyndb-ldap

#### 🔹 Run FreeIPA Installer

   sudo ipa-server-install --setup-dns

Key Configuration Choices:

* Server Hostname: idm-primary.lab.local
* Domain: lab.local
* Realm: LAB.LOCAL
* Passwords: Directory Manager & IPA admin (secure strong passwords)
* Integrated DNS: Enabled with Google (8.8.8.8) and Cloudflare (1.1.1.1) forwarders
* NetBIOS: Default LAB
* Confirm configuration: Yes

Ensures centralized authentication and DNS integration for the lab environment.

### 4️⃣ FreeIPA Clients & Replica Setup

Replicas configured as FreeIPA replica servers, synchronized with idm-primary.

Clients can be enrolled using ipa-client-install for secure access and centralized authentication.

### 5️⃣ Identity & Access Management (IAM) Configuration

#### 🔹 Groups

* admins — full administrative access
* devs — development team
* finance — finance team

#### 🔹 Users

* carol_admin → admins
* alice_dev → devs
* bob_finance → finance

#### 🔹 Sudo & HBAC Rules

* SUDO: Full access for admins group
* HBAC: Host-based access control rules limiting SSH access per user and host

#### 🔹 Kerberos Authentication

* Ticket-based authentication enabled across all nodes
* Users authenticate securely using Kerberos principal

### 6️⃣ Secure Management

* SSH Key Authentication for all servers
* HTTPS Web UI accessible via SSH tunnel for secure remote administration

Ensures enterprise-grade security for administrative access


## Key Achievements

* **Cloud Infrastructure & IaC:** Architected and deployed a **custom VPC, subnet, firewall rules, and virtual machines** using **GCP CLI**, demonstrating **Infrastructure as Code principles**.
* **Identity & Access Management (IAM):** Implemented a **centralized authentication and authorization system** with FreeIPA, including **Kerberos authentication**, **host enrollment**, **user/group policies**, and **access control rules**.
* **Linux System Administration:** Configured **hostnames, FQDNs, /etc/hosts entries**, performed **package management** (`dnf`), and set up **host firewalld rules**, ensuring secure system baselines.
* **Secure Management Practices:** Enabled **SSH key authentication** and **HTTPS-based administration** via **SSH tunnels**, demonstrating secure remote management of critical systems.
* **Networking & Security:** Configured **private network IPs**, **host-based access control (HBAC)**, **firewall rules**, and **SUDO policies**, enforcing **least privilege principles**.
* **Automation & CLI Proficiency:** Leveraged **gcloud**, `ipa-server-install`, and `ipa-client-install` commands for **repeatable, automated deployments**, reducing manual configuration errors.

## 🔹 Technologies & Security Concepts Utilized

* **Google Cloud Platform (GCP):** VPC, Subnet, Firewall, VM instances
* **FreeIPA:** Identity management, Kerberos authentication, DNS integration
* **Linux (CentOS Stream 9):** System configuration, service management, package updates
* **Security Tools & Concepts:** SSH, HTTPS, SUDO rules, HBAC, Kerberos tickets, access control
* **Infrastructure as Code (IaC):** Command-line automation with **repeatable deployment scripts**

## 🔹 Project Impact

* Demonstrates a **secure multi-server IdM environment** suitable for enterprise or lab testing
* Provides a **repeatable blueprint** for deploying **FreeIPA on cloud infrastructure**
* Strengthens understanding of **cloud networking, identity management, and Linux security best practices**
* Illustrates **integration of cloud, security, and automation skills** in a professional project