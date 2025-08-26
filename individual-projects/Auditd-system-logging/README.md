# Linux System Auditing with auditd

## Project Overview
This project demonstrates the practical application of the Linux Auditing System to monitor for security-related events. We will configure the `auditd` daemon to track unauthorized file changes, simulate a failed access attempt, and then use `ausearch` to analyze the security log, proving the system's ability to act as a forensic tool.

***

## The Tool: Linux Auditing System

### What is it?
The **Linux Auditing System** is a security framework that records security-relevant events on a Linux system. It's a critical component for **security monitoring**, **compliance**, and **forensics**, acting like a "flight recorder" to track what happens on a system, when it happened, and who was responsible.

### Key Components
* `auditd`: The core daemon that runs in the background and writes events to the logs.
* `auditctl`: A command-line tool used to add or remove temporary audit rules.
* `ausearch`: A powerful utility for filtering and searching through the `auditd` logs.
* `aureport`: A tool for generating summary reports from the logs.

### Key Files
* `/var/log/audit/audit.log`: The main log file where all security events are stored.
* `/etc/audit/auditd.conf`: The main configuration file for the `auditd` daemon.
* `/etc/audit/rules.d/`: The directory for storing persistent audit rules that will survive a reboot.

***

## Methodology and Project Steps

### Step 1: Confirm `auditd` Service Status
We first verified that the `auditd` service was running correctly.

   sudo systemctl status auditd

### Step 2: Configure a Watch Rule
We created a new file and then used `auditctl` to add a rule to monitor it. We used the `-w` flag to specify the file, `-p wa` to watch for write and attribute changes, and `-k project_file_monitor` to add a unique key for easy searching.

   sudo touch /tmp/important_file.txt
   sudo auditctl -w /tmp/important_file.txt -p wa -k project_file_monitor
   sudo auditctl -l

### Step 3: Trigger an Audit Event
To test our rule, we attempted to write to the file. This action was intentionally performed without proper permissions to demonstrate `auditd`'s ability to log both successful and unsuccessful attempts.

   sudo echo "This is a test." >> /tmp/important_file.txt

*Note: This command failed due to a common `sudo` redirection issue, but `auditd` correctly logged the failed attempt.*

### Step 4: Analyze the Log with `ausearch`
Finally, we used `ausearch` with our custom key (`-k project_file_monitor`) to find the event in the audit logs.

   sudo ausearch -k project_file_monitor

***

## Project Analysis and Results
The `ausearch` output confirmed that `auditd` logged the failed write attempt. This is a critical result because it shows the system's ability to act as an immediate and detailed alert system.
* `type=SYSCALL`: Indicates a system call was attempted.
* `success=no`: Confirms the operation was a failure.
* `exit=-13`: The kernel's exit code for "Permission denied," which perfectly matches the error we received.
* `uid=1000`: Identifies the user who attempted the action.
* `key="project_file_monitor"`: The unique identifier we created, which allowed for quick and efficient log analysis.

This project successfully demonstrated how to use `auditd` and `ausearch` to proactively monitor a system and retrospectively investigate security events. It proves a foundational skill in both proactive security and incident response.