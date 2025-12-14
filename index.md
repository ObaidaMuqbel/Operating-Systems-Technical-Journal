# CMPN202 – Operating Systems  
## Technical Journal  

**Name:** Obaida Muqbel  
**Student ID:** A00013570  
**Module:** CMPN202 Operating Systems  

---

## Phase 1 – System Planning & Distribution Selection

### 1.1 System Architecture
The system follows a dual-machine architecture. An Ubuntu Server (headless) acts as the managed server, while an Ubuntu Desktop virtual machine acts as the workstation. All administration is performed remotely over SSH, enforcing command-line proficiency and reflecting real-world server management practices.

**Architecture Description:**
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

**IP Configuration Evidence:**
- Server IP: `10.0.2.15/24`
- Network Interface: `enp0s3`

Connectivity was verified using ICMP:
```bash
ping -c 10 google.com
