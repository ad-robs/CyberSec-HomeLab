# CyberSec-HomeLab
This project is a hands-on cybersecurity home lab built using 7 virtual machines, designed to simulate a real-world enterprise environment. The lab includes an **Active Directory Domain**, an **E-Mail and Corporate Server**, a **Security Server**, an **attacker machine** and monitoring/detection tools using **Wazuh** to practice blue and red team skills.

## Project Overview
-**Goal:** Build and secure a simulated company network, configuring DNS, DHCP, Active Directory and a SIEM (Wazuh), then test defences with an attack.
-**Environment:** Virtual machines running on VirtualBox
**Focus Areas:**
-Windows Active Directory setup and user management
-Configuring Wazuh and setting up detection alerts
-Attacking with Kali Linux (privilege escalation, lateral movement, data exfiltration, persistence)

## Lab Architecture
**Tools/Technology Used:**
| Tool / Technology       | Purpose |
|--------------------------|---------|
| **VirtualBox**           | Virtualization platform to host all VMs |
| **Windows Server 2022**  | Domain Controller for AD, DNS, DHCP |
| **Windows 11**           | Domain-joined client workstation |
| **Ubuntu 22.04**         | Linux client & corporate email server (MailHog) |
| **Kali Linux**           | Attacker machine (offensive tools & exploits) |
| **Wazuh (SIEM)**         | Log monitoring, detection & alerting |
| **Security Onion**       | Blue team analysis workstation (future use) |
| **Hydra**                | Brute-force attack tool for SSH |
| **Evil-WinRM**           | Remote access tool for Windows targets |
| **NetExec**              | Lateral movement & credential spraying |
| **Nmap**                 | Reconnaissance & port scanning |
| **MailHog**              | Fake email server for phishing practice |


**Components:**
-**Domain Controller (Windows Server 2022)**-> Active Directory, DNS, DHCP
-**Windows Client (Windows 11)**-> Client with wazuh agent
-**Linux Client (Ubuntu 22.04)**-> Client with wazuh agent
-**Corp-Server (Ubuntu 22.04)**-> E-Mail Server using MailHog
-**Security Onion Workstation (Oracle Linux)**->Part of domain, future blue team work
-**Security Box(Ubunutu 22.04)**-> Hosted Wazuh
-**Attacker Machine (Kali Linux)**-> Attacked corporate network

## Setup Steps
1. Installed VirtualBox and configured all machines above.
2. Configured Domain Controller (users, groups).
3. Joined all machines bar the attacker machine to the domain.
4. Set up e-mail server using mailhog.
5. Setup Wazuh on the Security Box machine, installing agents on the windows client, linux client and the domain controller.
6. Created a vulnerable environment (Opened port 22 on email server for SSH, used simple passwords).
7. Set up detections and alerts (file integrity monitoring, sshd authentication fails and WinRM logins.
8. Began Reconaissance with nmap port scan, used hydra to brute force ssh using rockyou.txt wordlist, into the corp-server machine.
9. Checked for any services running and find mailhog running, gain access to mailhog and find emails from other users in the domain.
10. Set up static website which captures username and password. Send phishing email to user logged in to linux client.
11. SSH into victims machine using provided credentials. Perform some basic reconaissance (OS Version, host name, ip address).
12. Performed port scan using nmap, discovered WinRM running. Performed password spraying using NetExec and capture Administrator username + password.
13. Use evil-winrm to gain a shell into the domain controller using the captured credentials.
14. Discover RDP running and launch a RDP session using xfreerdp.
15. Exfiltrate file made called 'secrets' using scp command line program back to the attacker machine.
16. Gain persistence by creating a new user account and grant administrator privileges. Also create a scheduled task which will run a backdoor every 24 hours (Chatgpt used to make script).
17. Open a listener on port 4444 on attacker machine at 12 pm and catch a reverse shell.
18. Look at alerts and logs genarated on wazuh.

## Attack Chain Summary (MITRE ATT&CK)
-**Reconaissance**: Nmap Scans to identify open services (22-SSH, 3389-RDP, 5985-WinRM).
-**Initial Access**: SSH Brute force attack on corp server using Hydra
-**Execution**: Exploting weak credentials on Mailhog, and phishing the user on linux client machine.
-**Persistence**: Created new admin user + scheduled task backdoor.
-**Privilege Escalation**: Captured domain admin credentials using evil-winrm.
-**Credential Access**: Harvested credentials from phishing + spraying.
-**Lateral Movement**: Exploited WinRM to move from linux client to domain controller.
-**Exfiltration**- Copied 'secrets.txt' file via SCP to attacker machine.
-**Command & Control**- Reverse shell callback using listener on port 4444.

## Lessons Learned
-Setting up Active Directory manually gave me real experience with domain users, groups, DNS and DHCP.
-Saw firsthand how weak passwords and open ports create quick entry points.
-Practiced offensive skills like brute force, phishing, lateral movement, and persistence.
-Learned how a SIEM like Wazuh can detect brute force, failed logins, and suspicious processes.
-Gained visibility into how attackers move through a network once they compromise one machine.
-Built confidence in simulating both red team attacks and blue team detection/response.

