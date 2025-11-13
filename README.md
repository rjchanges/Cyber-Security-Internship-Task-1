# Cyber-Security-Internship-Task-1
# Task 1: Network Scanning with Nmap

## Objective
To discover open ports on devices in the local network (`192.168.1.0/24`) and understand network service exposure.

## Steps Taken
1. **Tool Installation:** Installed Nmap 7.98 on Windows.
2. **Discovery:** Identified local IP range as `192.168.1.0/24`.
3. **Scanning:** Performed a comprehensive scan using `nmap -A` (Aggressive scan for OS and Version detection).
4. **Analysis:** Analyzed the output to identify 3 active hosts.

## Findings & Analysis

### Host 1: 192.168.1.1 (Network Router)
- **OS/Service:** Linux 2.6.x / Boa HTTPd.
- **Open Ports:**
  - **Port 80 (HTTP):** Router configuration page.
  - **Port 53 (DNS):** dnsmasq service.
  - **Port 56090 (Telnet):** **CRITICAL FIND.** An unencrypted Telnet service is running on a high port. This poses a security risk as credentials sent over Telnet can be intercepted in plain text.

### Host 2: 192.168.1.4 (Windows Workstation)
- **OS:** Microsoft Windows.
- **Open Ports:**
  - **Port 445 (SMB):** Windows File Sharing.
  - **Port 135/139:** RPC and NetBIOS.
  - **Ports 49664-49669:** Dynamic RPC ports used by Windows services.
- **Security Note:** Port 445 is standard for local networks but must be firewalled from the public internet to prevent SMB exploits.

### Host 3: 192.168.1.5
- **Status:** Host is up but all ports are closed/filtered.
- **Analysis:** Likely a mobile device or IoT device with a strict firewall policy.

## Interview Questions & Answers
**1. What is an open port?**
An open port is a network endpoint (identified by a number) that is actively listening for incoming connections from other devices.

**2. How does Nmap perform a TCP SYN scan?**
It sends a SYN packet. If the port is open, the target replies with SYN/ACK. Nmap then immediately sends a RST (Reset) to cancel the connection, making the scan faster and stealthier than a full connection.

**3. What risks are associated with open ports?**
Open ports increase the "attack surface." For example, the Telnet port found on `192.168.1.1` could allow an attacker to capture passwords, unlike SSH which is encrypted.

**4. How can open ports be secured?**
- **Firewalls:** Configure rules to block access to sensitive ports (like 445 or 56090) from unauthorized IPs.
- **Disable Services:** If a service (like Telnet) is not needed, turn it off completely.
- **Patching:** Keep the software running on the port (e.g., Boa HTTPd) updated to fix known vulnerabilities.
