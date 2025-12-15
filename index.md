<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CMPN202 – Operating Systems Technical Journal</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      line-height: 1.6;
      margin: 20px;
      max-width: 900px;
    }
    h1, h2, h3, h4 {
      border-bottom: 1px solid #ccc;
      padding-bottom: 4px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      overflow-x: auto;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin: 10px 0;
    }
    table, th, td {
      border: 1px solid #ccc;
    }
    th, td {
      padding: 8px;
      text-align: left;
    }
    .section {
      margin-bottom: 40px;
    }
  </style>
</head>
<body>

<h1>CMPN202 – Operating Systems</h1>
<h2>Technical Journal</h2>
<p><strong>Name:</strong> Obaida Muqbel</p>
<p><strong>Student ID:</strong> A00013570</p>
<p><strong>Module:</strong> CMPN202 Operating Systems</p>

<div class="section">
  <h2>Phase 1 – System Planning & Distribution Selection</h2>

  <h3>1.1 System Architecture</h3>
  <p>The system is deployed using a virtualised multi-machine environment on a Windows 11 host. Two Linux virtual machines are used, with all administration performed remotely via SSH to enforce command-line proficiency and reflect real-world server management practices.</p>
  <p><strong>Architecture Description:</strong> Oracle VirtualBox is used as a Type-2 hypervisor to host Ubuntu and Linux Mint virtual machines. Each VM operates as an isolated guest operating system and is connected through a private VirtualBox NAT network.</p>

  <h3>1.2 Distribution Selection Justification</h3>
  <p>Two Linux distributions were evaluated: Ubuntu Server and Linux Mint.</p>
  <ul>
    <li><strong>Ubuntu Server:</strong> Selected for stability, long-term support (LTS), strong community support, cloud/data centre relevance, headless operation efficiency.</li>
    <li><strong>Linux Mint:</strong> User-friendly desktop, stable for long-term desktop use, limited server tooling and documentation.</li>
  </ul>

  <h3>1.3 Workstation Selection</h3>
  <p>The workstation system is an Ubuntu Desktop VM, providing a graphical environment for documentation while allowing full SSH-based administration of the server.</p>

  <h3>1.4 Network Configuration</h3>
  <p>Both VMs are configured using VirtualBox NAT networking for outbound internet access while preventing unsolicited inbound connections.</p>
  <pre>
IP Configuration Evidence:
Server IP: 10.0.2.15/24
Network Interface: enp0s3

Connectivity verified using ICMP:
ping -c 10 google.com
Ubuntu: 10 packets received, 0% packet loss, time 13455ms
Mint: 10 packets received, 0% packet loss, time 9169ms
  </pre>

  <h3>1.5 System Specification Evidence</h3>
  <pre>
Commands executed via CLI:
uname -a
free -h
df -h
ip addr
lsb_release -a
  </pre>
</div>

<div class="section">
  <h2>Phase 2 – Security Planning & Testing Methodology</h2>
  
  <h3>2.1 Performance Testing Plan</h3>
  <p>Testing conducted remotely via SSH. Metrics collected during CPU, memory, disk, and network stress tests to evaluate OS behavior.</p>

  <h3>2.2 Security Configuration Checklist</h3>
  <ul>
    <li>SSH hardening (key-based authentication, no root login)</li>
    <li>Firewall restricting SSH access to a single workstation IP</li>
    <li>Mandatory Access Control (AppArmor)</li>
    <li>Automatic security updates</li>
    <li>User privilege management (non-root admin user)</li>
    <li>Network monitoring and logging</li>
  </ul>

  <h3>2.3 Threat Model</h3>
  <table>
    <tr>
      <th>Threat</th>
      <th>Description</th>
      <th>Mitigation</th>
    </tr>
    <tr>
      <td>Malware</td>
      <td>Exploits vulnerabilities to gain control</td>
      <td>Automatic updates, limited sudo access</td>
    </tr>
    <tr>
      <td>DoS Attack</td>
      <td>Resource exhaustion</td>
      <td>Firewall rules, monitoring</td>
    </tr>
    <tr>
      <td>SSH Brute Force</td>
      <td>Credential guessing</td>
      <td>SSH keys, fail2ban, login attempt limits</td>
    </tr>
  </table>
</div>

<div class="section">
  <h2>Phase 3 – Application Selection for Performance Testing</h2>

  <h3>3.1 Application Selection Matrix</h3>
  <table>
    <tr>
      <th>Workload Type</th>
      <th>Application</th>
      <th>Justification</th>
    </tr>
    <tr>
      <td>CPU</td>
      <td>sysbench</td>
      <td>Industry-standard CPU benchmarking tool</td>
    </tr>
    <tr>
      <td>RAM</td>
      <td>stress-ng</td>
      <td>Controlled memory stress testing</td>
    </tr>
    <tr>
      <td>Disk</td>
      <td>dd</td>
      <td>Low-level disk I/O measurement</td>
    </tr>
    <tr>
      <td>Network</td>
      <td>iperf3</td>
      <td>Bandwidth and latency testing</td>
    </tr>
    <tr>
      <td>Server</td>
      <td>Apache</td>
      <td>Real-world multi-client workload</td>
    </tr>
  </table>

  <h3>3.2 Installation Documentation (via SSH)</h3>
  <pre>
sudo apt update && sudo apt upgrade -y
sudo apt install sysbench stress-ng iperf3 apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
  </pre>
</div>

<div class="section">
  <h2>Phase 4 – Initial System Configuration & Security Implementation</h2>
  <h3>4.1 SSH Hardening</h3>
  <p>SSH configured for key-based authentication. Root login disabled by editing <code>/etc/ssh/sshd_config</code>.</p>
  <pre>
Before:

After:

Successful SSH login:
  </pre>

  <h3>4.2 Firewall Configuration</h3>
  <pre>
sudo ufw allow from &lt;workstation-ip&gt; to any port 22
sudo ufw enable
sudo ufw status verbose
  </pre>

  <h3>4.3 User Privilege Management</h3>
  <pre>
sudo adduser adminuser
sudo usermod -aG sudo adminuser
  </pre>
</div>

<div class="section">
  <h2>Phase 5 – Advanced Security & Monitoring Infrastructure</h2>
  <h3>5.1 Mandatory Access Control (AppArmor)</h3>
  <pre>
aa-status
  </pre>

  <h3>5.2 Automatic Updates</h3>
  <pre>
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
  </pre>

  <h3>5.3 Intrusion Detection (fail2ban)</h3>
  <pre>
fail2ban installation and SSH protection configured
  </pre>

  <h3>5.4 Security Baseline Script</h3>
  <pre>
security-baseline.sh verifies SSH, firewall, fail2ban, AppArmor, and updates
  </pre>

  <h3>5.5 Remote Monitoring Script</h3>
  <pre>
monitor-server.sh runs on workstation to collect CPU, RAM, disk, network metrics
  </pre>
</div>

<div class="section">
  <h2>Phase 6 – Performance Evaluation & Analysis</h2>
  <h3>6.2 Performance Data & Visualisation</h3>
  <table>
    <tr>
      <th>Subsystem</th>
      <th>Tool</th>
      <th>Metric Observed</th>
      <th>Result</th>
    </tr>
    <tr>
      <td>CPU</td>
      <td>sysbench</td>
      <td>Events per second</td>
      <td>~10,900 ops/s</td>
    </tr>
    <tr>
      <td>Memory</td>
      <td>sysbench</td>
      <td>Memory throughput</td>
      <td>~41 GB/s</td>
    </tr>
    <tr>
      <td>Disk</td>
      <td>dd</td>
      <td>Sequential write throughput</td>
      <td>~446 MB/s</td>
    </tr>
    <tr>
      <td>Network</td>
      <td>Iperf3</td>
      <td>TCP bandwidth</td>
      <td>~3.8-4.0 Gbit/s</td>
    </tr>
    <tr>
      <td>Server</td>
      <td>siege</td>
      <td>Transactions per second</td>
      <td>~3,700 tx/s</td>
    </tr>
  </table>
</div>

<div class="section">
  <h2>Phase 7 – Security Audit & System Evaluation</h2>
  <h3>7.1 Security Audit</h3>
  <pre>
sudo lynis audit system
  </pre>
  <p>Hardening index: 63 – Core security controls successfully implemented.</p>

  <h3>7.2 Network Security Assessment</h3>
  <p>Nmap scan confirmed only SSH accessible.</p>

  <h3>7.3 Service Audit</h3>
  <pre>
systemctl list-units --type=service --state=running
  </pre>

  <h3>7.4 Final Evaluation</h3>
  <p>The system demonstrates strong security posture, efficient resource usage, and professional Linux server administration practices.</p>
</div>

<div class="section">
  <h2>References</h2>
  <ul>
    <li>ChatGPT – Helped with grammatical and English errors throughout the document for professional look.</li>
  </ul>
</div>

</body>
</html>
