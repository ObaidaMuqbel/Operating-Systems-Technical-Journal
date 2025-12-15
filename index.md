# CMPN202 – Operating Systems
## Technical Journal

**Name:** Obaida Muqbel  
**Student ID:** A00013570  
**Module:** CMPN202 Operating Systems  


## Phase 1 – System Planning & Distribution Selection

### 1.1 System Architecture
The system uses a dual-machine architecture: an Ubuntu Server (headless) and an Ubuntu Desktop VM as the workstation. Administration is performed remotely via SSH.

**Architecture Description:**
- **Workstation:** Ubuntu Desktop VM  
- **Server:** Ubuntu Server (headless)  
- **Network:** VirtualBox NAT  
- **Remote Access:** SSH (TCP port 22)  

### 1.2 Distribution Selection Justification
Two distributions were evaluated: Ubuntu Server and Linux Mint.  
- **Ubuntu Server:** Chosen for stability, LTS support, and headless server suitability.  
- **Linux Mint:** Usable desktop but unsuitable for server deployment.  

### 1.3 Workstation Selection
Ubuntu Desktop VM provides SSH compatibility and a graphical environment for documentation.

### 1.4 Network Configuration
Both VMs use VirtualBox NAT networking.

**IP Configuration**
Server IP: 10.0.2.15/24
Interface: enp0s3

Connectivity test:
ping -c 10 google.com
Result: 0% packet loss on both systems

powershell
Copy code

### 1.5 System Specification Evidence
System information collected via CLI:
uname -a
free -h
df -h
ip addr
lsb_release -a

yaml
Copy code

---

## Phase 2 – Security Planning & Testing Methodology

### 2.1 Performance Testing Plan
Testing conducted remotely via SSH with baseline and load-based metrics.

### 2.2 Security Configuration Checklist
- SSH key-based authentication  
- Firewall restricted to workstation IP  
- AppArmor enabled  
- Automatic updates  
- Non-root administrative user  
- Network logging  

### 2.3 Threat Model

| Threat        | Description              | Mitigation              |
|---------------|-------------------------|------------------------|
| Malware       | Exploits vulnerabilities| Automatic updates      |
| DoS Attack    | Resource exhaustion     | Firewall rules         |
| SSH Brute Force| Credential guessing    | SSH keys, fail2ban     |

---

## Phase 3 – Application Selection for Performance Testing

### 3.1 Application Selection Matrix

| Workload | Application | Justification       |
|----------|------------|------------------|
| CPU      | sysbench   | CPU benchmarking |
| RAM      | stress-ng  | Memory stress    |
| Disk     | dd         | Disk I/O testing |
| Network  | iperf3     | Bandwidth testing |
| Server   | Apache     | Real workload    |

### 3.2 Installation Documentation
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install sysbench stress-ng iperf3 apache2 -y

# Start Apache
sudo systemctl enable apache2
sudo systemctl start apache2
3.3 Monitoring Strategy
top, htop – CPU/RAM

iotop – Disk

iftop – Network

journalctl – Logs

Phase 4 – Initial System Configuration & Security
4.1 SSH Hardening
SSH configured for key-based authentication and root login disabled.

4.2 Firewall Configuration
bash
Copy code
sudo ufw allow from <workstation-ip> to any port 22
sudo ufw enable
sudo ufw status verbose
4.3 User Privilege Management
bash
Copy code
sudo adduser adminuser
sudo usermod -aG sudo adminuser
Phase 5 – Advanced Security
5.1 AppArmor Verification
bash
Copy code
aa-status
5.2 Automatic Updates
bash
Copy code
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
5.3 Fail2Ban
Configured to protect SSH from brute-force attacks.

Phase 6 – Performance Evaluation
6.2 Performance Results
Subsystem	Tool	Metric	Result
CPU	sysbench	Events/sec	~10,900
Memory	sysbench	Throughput	~41 GB/s
Disk	dd	Write speed	~446 MB/s
Network	iperf3	Bandwidth	~4 Gbit/s

6.3 Trade-off Analysis
Security controls introduced minimal overhead while improving system resilience.

Phase 7 – Security Audit
7.1 Lynis Audit
bash
Copy code
sudo lynis audit system
Hardening index: 63

7.2 Final Evaluation
The system demonstrates secure configuration, efficient performance, and professional Linux administration practices.

References
ChatGPT – Grammar and clarity assistance
