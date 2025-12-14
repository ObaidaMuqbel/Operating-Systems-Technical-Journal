# CMPN202 – Operating Systems  
## Technical Journal  

**Name:** Obaida Muqbel  
**Student ID:** A00013570  
**Module:** CMPN202 Operating Systems  

> This GitHub Pages site documents my CMPN202 technical journal across all seven phases.  
> Detailed screenshots, performance graphs, and command output are provided in the submitted technical journal document.

---

## Phase 1 – System Planning & Distribution Selection

### 1.1 System Architecture
The system follows a dual-machine architecture. An Ubuntu Server (headless) acts as the managed server, while an Ubuntu Desktop virtual machine acts as the workstation. All administration is performed remotely over SSH, enforcing command-line proficiency and reflecting real-world server management practices.

**Architecture Description**
- Workstation: Ubuntu Desktop VM  
- Server: Ubuntu Server (headless)  
- Network: VirtualBox NAT  
- Remote Access: SSH (TCP port 22) from workstation to server only  

---

### 1.2 Distribution Selection Justification
Two Linux distributions were evaluated: Ubuntu Server and Linux Mint.

Ubuntu Server was selected for the server system due to its stability, extensive long-term support (LTS), and strong community and enterprise backing. Ubuntu Server is widely used in cloud and data centre environments, making it ideal for learning professional system administration skills. Its headless operation encourages efficient resource usage and aligns with sustainability principles.

Linux Mint was considered due to its user-friendly desktop environment and stability for long-term desktop use. However, Mint is not designed for server deployments and offers limited tooling and documentation for headless server administration.

---

### 1.3 Workstation Selection
The workstation system is an Ubuntu Desktop virtual machine. This choice provides a familiar graphical environment for documentation while allowing full SSH-based administration of the server. Using a Linux workstation ensures native compatibility with SSH, monitoring tools, and scripting utilities.

---

### 1.4 Network Configuration
Both virtual machines are configured using VirtualBox NAT networking. NAT allows outbound internet access for updates while preventing unsolicited inbound connections from external networks, improving security.

**IP Configuration Evidence**
- Server IP: `10.0.2.15/24`
- Network Interface: `enp0s3`

Connectivity was verified using ICMP:
```bash
ping -c 10 google.com
Both systems reported 0% packet loss, confirming reliable connectivity.

1.5 System Specification Evidence
The following commands were executed via the command line to document system specifications:

bash
Copy code
uname -a
free -h
df -h
ip addr
lsb_release -a
Screenshots of these commands are provided in the submitted technical journal document.

Phase 2 – Security Planning & Testing Methodology
2.1 Performance Testing Plan
Performance testing was conducted remotely from the workstation using SSH. Baseline measurements were recorded on an idle system, followed by controlled workload execution. Metrics were collected during CPU, memory, disk, and network stress to evaluate operating system behaviour under load.

2.2 Security Configuration Checklist
SSH hardening (key-based authentication, no root login)

Firewall restricting SSH access to a single workstation IP

Mandatory Access Control (AppArmor)

Automatic security updates

User privilege management (non-root administrative user)

Network monitoring and logging

2.3 Threat Model
Threat	Description	Mitigation
Malware	Exploits vulnerabilities	Automatic updates, limited sudo access
DoS Attack	Resource exhaustion	Firewall rules, system monitoring
SSH Brute Force	Credential guessing	SSH keys, fail2ban

Phase 3 – Application Selection for Performance Testing
3.1 Application Selection Matrix
Workload	Application	Justification
CPU	sysbench	Industry-standard benchmarking
RAM	stress-ng	Controlled memory stress
Disk	dd	Low-level disk I/O
Network	iperf3	Bandwidth testing
Server	Apache	Real-world workload

3.2 Installation Documentation
Applications were installed remotely via SSH:

bash
Copy code
sudo apt update && sudo apt upgrade -y
sudo apt install sysbench stress-ng iperf3 apache2 -y
Apache was enabled and started:

bash
Copy code
sudo systemctl enable apache2
sudo systemctl start apache2
3.3 Expected Resource Profiles
Each application stresses a specific subsystem, allowing accurate performance analysis and bottleneck identification.

3.4 Monitoring Strategy
Monitoring tools used include:

top, htop – CPU & RAM

iotop – Disk I/O

iftop – Network traffic

vmstat, free – Memory

journalctl – Logs

Phase 4 – Initial System Configuration & Security Implementation
4.1 SSH Hardening
SSH was configured for key-based authentication. Password authentication and direct root login were disabled in /etc/ssh/sshd_config.

4.2 Firewall Configuration
UFW was configured to allow SSH only from the workstation:

bash
Copy code
sudo ufw allow from <workstation-ip> to any port 22
sudo ufw enable
sudo ufw status verbose
4.3 User Privilege Management
A non-root administrative user was created:

bash
Copy code
sudo adduser adminuser
sudo usermod -aG sudo adminuser
4.4 Remote Administration Evidence
All configuration tasks were performed via SSH, demonstrating exclusive command-line administration.

Phase 5 – Advanced Security & Monitoring Infrastructure
5.1 Mandatory Access Control (AppArmor)
AppArmor profiles were verified using:

bash
Copy code
aa-status
5.2 Automatic Updates
Unattended upgrades were enabled:

bash
Copy code
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
5.3 Intrusion Detection (fail2ban)
Fail2Ban was installed and configured to protect SSH.

5.4 Security Baseline Script
The security-baseline.sh script verifies SSH configuration, firewall status, Fail2Ban, AppArmor, and update services.

5.5 Remote Monitoring Script
The monitor-server.sh script collects system metrics remotely via SSH.
Fail2Ban status checks were prioritised during evaluation.

Phase 6 – Performance Evaluation & Analysis
6.1 Testing Methodology
Baseline and load tests were conducted for each workload. Values were derived from representative test runs.

6.2 Performance Data & Visualisation
Subsystem	Tool	Metric	Result
CPU	sysbench	Events/sec	~10,900
Memory	sysbench	Throughput	~41 GB/s
Disk	dd	Write speed	~446 MB/s
Network	iperf3	Bandwidth	~3.8–4.0 Gbit/s
Server	siege	Transactions/sec	~3,700

6.3 Trade-off Analysis
Security controls introduced minimal overhead while significantly improving resilience and reducing attack surface.

Phase 7 – Security Audit & System Evaluation
7.1 Security Audit
A full audit was conducted using Lynis:

bash
Copy code
sudo lynis audit system
The hardening index of 63 reflects implemented security controls and acceptable risks within a controlled environment.

7.2 Network Security Assessment
Only SSH was accessible from the workstation, consistent with firewall rules.

7.3 Service Audit
Running services were reviewed using:

bash
Copy code
systemctl list-units --type=service --state=running
SSH service status was verified.

7.4 Final Evaluation
The system demonstrates secure configuration, efficient resource usage, and professional Linux administration practices. Trade-offs between performance and security were identified and justified.

References
ChatGPT – Assisted with grammar and clarity improvements.
