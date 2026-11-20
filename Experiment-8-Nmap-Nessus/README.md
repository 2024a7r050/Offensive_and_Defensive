# Experiment 8: Nmap and Nessus

## Aim
To identify open ports and running services using Nmap and detect known vulnerabilities using Nessus.

## Requirements
- Kali Linux
- Metasploitable 2
- Oracle VirtualBox
- Nmap
- Nessus
- Metasploit Framework (for the additional captured demonstration)

## Theory
**Nmap (Network Mapper)** is a network reconnaissance tool used to discover active hosts, open ports, and running services.

**Nessus** is a vulnerability-scanning tool used to identify known security weaknesses in a system.

The screenshots also document a controlled Metasploit demonstration against the deliberately vulnerable `vsftpd 2.3.4` service on Metasploitable 2, followed by a review of MITRE ATT&CK reference pages.

## Procedure

### 1. Check Kali network interfaces and target connectivity
The screenshot shows Kali's network interfaces and a successful ping to `192.168.1.100`.

![Kali network interfaces and ping test](screenshots/01-kali-network-and-connectivity.png)

### 2. Start Metasploit Framework
Launch Metasploit from the Kali terminal.

```bash
msfconsole
```

![Metasploit Framework startup](screenshots/02-metasploit-framework-startup.png)

### 3. Scan the target with Nmap
Run service and operating-system detection against the lab target.

```bash
nmap -sS -sV -O 192.168.1.100
```

The captured scan reports the target as active and lists open ports and detected services, including FTP (`21/tcp`) with `vsftpd 2.3.4`.

![Nmap service and OS detection output](screenshots/03-nmap-service-scan.png)

### 4. Review the vsftpd 2.3.4 Metasploit module
The screenshot documents selection of the `vsftpd_234_backdoor` module and the resulting lab session.

![Metasploit vsftpd exploit session](screenshots/04-vsftpd-exploit-session.png)

### 5. Review module and payload options
The screenshot records the target host (`RHOSTS`), FTP port (`RPORT`), and reverse Meterpreter payload settings.

![Exploit module options](screenshots/05-exploit-module-options.png)

### 6. Verify target system information
The Meterpreter output identifies the target as `metasploitable.localdomain`, running Ubuntu 8.04 with Linux kernel `2.6.24-16-server` on i686 architecture.

![Meterpreter system information](screenshots/06-meterpreter-system-information.png)

### 7. Review MITRE ATT&CK reference material
The following screenshots show the MITRE ATT&CK Enterprise Matrix and reference pages used to study attacker tactics and techniques.

![MITRE ATT&CK Enterprise Matrix](screenshots/07-mitre-attck-enterprise-matrix.png)

![MITRE ATT&CK Exploit Public-Facing Application](screenshots/08-mitre-exploit-public-facing-application.png)

![MITRE ATT&CK Defense Impairment](screenshots/09-mitre-defense-impairment.png)

## Result
Nmap identified the lab target's open ports and running services. The screenshots also document a controlled Metasploit demonstration involving the vulnerable `vsftpd 2.3.4` service and verification of target system information.

## Conclusion
The experiment demonstrated how Nmap can be used for host, port, and service discovery. Nessus is used for vulnerability assessment. The additional Metasploit and MITRE ATT&CK screenshots illustrate how a discovered vulnerable service can be studied in an isolated training environment and related to security techniques.

## Safety Note
Perform scanning and exploitation only on systems you own or have explicit permission to test. Keep Metasploitable 2 on an isolated host-only/lab network; do not expose this intentionally vulnerable machine to the internet or an untrusted network.

## Screenshots
All screenshots are stored in `screenshots/` and embedded above using relative paths.
