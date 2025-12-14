CMPN202 – Operating Systems
Technical Journal 
Name: Obaida Muqbel
Student ID: A00013570
Module: CMPN202 Operating Systems

















________________________________________
Phase 1 – System Planning & Distribution Selection
1.1 System Architecture
The system follows a dual-machine architecture. An Ubuntu Server (headless) acts as the managed server. An Ubuntu Desktop VM acts as the workstation. All administration is performed remotely over SSH, enforcing command-line proficiency and reflecting real-world server management practices.
Architecture Description :- Workstation (Ubuntu Desktop VM) - Server (Ubuntu Server – headless) - NAT Network (VirtualBox) - SSH (TCP Port 22) from workstation to server only
 






________________________________________
1.2 Distribution Selection Justification
Two Linux distributions were evaluated: Ubuntu Server and Linux Mint.
Ubuntu Server was selected for the server system due to its stability, extensive long-term support (LTS), and strong community and enterprise backing. Ubuntu Server is widely used in cloud and data centre environments, making it ideal for learning professional system administration skills. Its headless operation encourages efficient resource usage and aligns with sustainability principles.
Linux Mint was considered due to its user-friendly desktop environment and stability for long-term desktop use. However, Mint is not designed for server deployments and offers limited tooling and documentation for headless server administration.
________________________________________
1.3 Workstation Selection
The workstation system is an Ubuntu Desktop virtual machine. This choice provides a familiar graphical environment for documentation while allowing full SSH-based administration of the server. Using a Linux workstation ensures native compatibility with SSH, monitoring tools, and scripting utilities.
________________________________________
1.4 Network Configuration
Both virtual machines are configured using VirtualBox NAT networking. NAT allows outbound internet access for updates while preventing unsolicited inbound connections from external networks, improving security.
IP Configuration Evidence :- ip addr - Server IP: 10.0.2.15/24 - Network Interface: enp0s3
Connectivity was verified using ICMP:
ping -c 10 google.com
The results shown:
Ubuntu: 10 packets received, 0% packet loss, time 13455ms
                    rtt min/avg/max/mdev = 15.185/21.648/31.205/5.314
Mint: 10 packets received, 0% packet loss, time 9169ms
            Rtt min/avg/max/mdev = 13.310/27.791/115.178/29.441












 


________________________________________
1.5 System Specification Evidence
The following commands were executed via CLI to document system specifications:
uname -a
free -h
df -h
ip addr
lsb_release -a
Mint:







Ubuntu:


Ubuntu:








________________________________________
Phase 2 – Security Planning & Testing Methodology
2.1 Performance Testing Plan
Performance testing is conducted remotely from the workstation using SSH. Baseline measurements are recorded on an idle system, followed by controlled workload execution. Metrics are collected during CPU, memory, disk, and network stress to evaluate operating system behavior under load.
________________________________________
2.2 Security Configuration Checklist
•	SSH hardening (key-based authentication, no root login)
•	Firewall restricting SSH access to a single workstation IP
•	Mandatory Access Control (AppArmor)
•	Automatic security updates
•	User privilege management (non-root admin user)
•	Network monitoring and logging
________________________________________
2.3 Threat Model
Threat	Description	Mitigation
Malware	Exploits vulnerabilities to gain control	Automatic updates, limited sudo access
DoS Attack	Resource exhaustion	Firewall rules, monitoring
SSH Brute Force	Credential guessing	SSH keys, fail2ban, login attempt limits





________________________________________
Phase 3 – Application Selection for Performance Testing
3.1 Application Selection Matrix
Workload Type	Application	Justification
CPU	sysbench	Industry-standard CPU benchmarking tool
RAM	stress-ng	Controlled memory stress testing
Disk	dd	Low-level disk I/O measurement
Network	Iperf3	Bandwidth and latency testing
Server	Apache	Real-world multi-client workload

________________________________________
3.2 Installation Documentation (via SSH)
sudo apt update && sudo apt upgrade -y
sudo apt install sysbench stress-ng iperf3 apache2 -y
Apache was enabled and started using:
sudo systemctl enable apache2
sudo systemctl start apache2



  










 



 
 
________________________________________
3.3 Expected Resource Profiles
Each application is expected to stress a specific subsystem, allowing isolation of CPU, memory, disk, and network behaviour. This supports accurate performance analysis and bottleneck identification.

CPU-Intensive Workload – sysbench:






















Memory-Intensive Workload – stress-ng:

























Disk I/O Workload – dd:















________________________________________
3.4 Monitoring Strategy
Monitoring tools used include: - top / htop – CPU & RAM - iotop – Disk I/O - iftop – Network traffic - vmstat, free – Memory behaviour - journalctl – System logs
________________________________________
Phase 4 – Initial System Configuration & Security Implementation
4.1 SSH Hardening
SSH was configured to use key-based authentication. Password and root login were disabled by editing /etc/ssh/sshd_config.

Before / After:

Before:

















 

After:

























Successful SSH login:







________________________________________
4.2 Firewall Configuration
UFW was configured to allow SSH only from the workstation IP.
sudo ufw allow from <workstation-ip> to any port 22
sudo ufw enable
sudo ufw status verbose














________________________________________
4.3 User Privilege Management
A non-root administrative user was created and added to the sudo group.
sudo adduser adminuser
sudo usermod -aG sudo adminuser






















________________________________________
4.4 Remote Administration Evidence
All configuration tasks were performed via SSH. Screenshots show active SSH sessions executing administrative commands. 























________________________________________
Phase 5 – Advanced Security & Monitoring Infrastructure
5.1 Mandatory Access Control (AppArmor)
AppArmor is enabled by default on Ubuntu Server. Profiles were verified using:
aa-status

















































________________________________________
5.2 Automatic Updates
Unattended security updates were enabled:
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
 
 






________________________________________
5.3 Intrusion Detection (fail2ban)
fail2ban was installed and configured to protect SSH from brute-force attacks.
























________________________________________
5.4 Security Baseline Script
security-baseline.sh verifies SSH configuration, firewall status, fail2ban, AppArmor, and update services. The script includes comments, conditionals, and clear output.








 
 







	



















________________________________________
5.5 Remote Monitoring Script
monitor-server.sh runs on the workstation, connects via SSH, and collects CPU, RAM, disk, and network metrics.







(Fail2Ban status checked instead of running monitor-server.sh)















________________________________________
Phase 6 – Performance Evaluation & Analysis
6.1 Testing Methodology
Baseline, load, and optimization tests were conducted for each workload. Measurements were recorded and compared. Due to time and scope constraints, performance values were derived from representative test runs rather than repeated statistical sampling.
________________________________________
6.2 Performance Data & Visualisation
Tables and graphs illustrate CPU utilisation, memory consumption, disk throughput, and network bandwidth underload.
Subsystem	Tool	Metric Observed	Result
CPU	sysbench	Events per second	~10,900 ops/s
Memory	sysbench	Memory throughput	~41 GB/s
Disk	dd	Sequential write throughput	~446 MB/s
Network	Iperf3	TCP bandwidth	~3.8-4.0 Gbit/s
Server	siege	Transactions per second	~3,700 tx/s

 














CPU Perfomance Testing:












Memory Performance Testing:














Disk I/O Performance Testing:

 
Network Performance Testing:















 
Server Workload Performance:
















________________________________________
6.3 Trade-off Analysis
A clear trade-off was observed between system security and performance overhead. The use of firewall rules, intrusion detection, and mandatory access control introduced minor background processing costs; however, performance testing demonstrated that these controls did not significantly impact throughput or responsiveness. This trade-off was considered acceptable given the substantial improvement in system resilience and reduced attack surface.




________________________________________
Phase 7 – Security Audit & System Evaluation
7.1 Security Audit
A full security audit was conducted using Lynis:
sudo lynis audit system
The Lynis audit produced a hardening index of 63, indicating that several core security controls were successfully implemented, including firewall enforcement, SSH hardening, and mandatory access control. The score was reduced by the absence of a malware scanner and the presence of installed compilers, which were assessed as acceptable risks within a controlled, non-production virtualized environment.
 
 
________________________________________
7.2 Network Security Assessment
An Nmap scan from the workstation confirmed that only SSH was accessible.

User Privilege Review:






Network Exposure Review:
 

 
Mandatory Access Control Review (AppArmor):

























________________________________________
7.3 Service Audit
All running services were reviewed and justified:
systemctl list-units --type=service --state=running

















 
SSH Service Verification:










________________________________________
7.4 Final Evaluation
The system demonstrates strong security posture, efficient resource usage, and professional Linux server administration practices. Trade-offs between performance and security were identified, measured, and justified using quantitative evidence.
________________________________________

