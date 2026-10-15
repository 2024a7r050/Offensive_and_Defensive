# Experiment 5: ARP Spoofing and Session Hijacking

## Aim
To study ARP spoofing and session hijacking concepts by capturing and analyzing network traffic and examining session-cookie attributes in a controlled virtual lab.

## Objectives
- Verify the Kali Linux and Metasploitable 2 lab network configuration.
- Capture ARP requests and replies with Wireshark.
- Inspect IP-to-MAC address mappings.
- Generate and inspect HTTP traffic.
- Examine cookie security attributes and understand session-hijacking risks.

## Requirements
- Kali Linux
- Metasploitable 2
- Wireshark
- VMware Workstation or VirtualBox
- A web service on the isolated lab network

## Lab configuration
- Kali `eth0` (NAT): `192.168.10.128/24`
- Kali `eth1` (isolated lab): `192.168.44.130/24`
- Metasploitable 2: `192.168.44.129`
- pfSense LAN, if used: `192.168.44.254`

Use these steps only in the isolated lab or on systems for which you have explicit authorization.

## Procedure

### Step 1 — Network setup
On Kali, check interfaces and routes:
```bash
ip addr
ip route
```
Confirm that `eth1` is connected to the isolated lab network and that Metasploitable is reachable.

### Step 2 — Enable IPv4 forwarding
On Kali:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
cat /proc/sys/net/ipv4/ip_forward
```
Expected output from the second command: `1`.

### Step 3 — Capture ARP traffic
Open Wireshark, select `eth1`, start capturing, and apply the display filter:
```text
arp
```
Generate ordinary lab traffic:
```bash
ping -c 4 192.168.44.129
```
Inspect the ARP requests and replies.

### Step 4 — Inspect IP-to-MAC mappings
Run:
```bash
ip neigh
```
Compare the neighbor-table entries with the ARP packets captured in Wireshark.

### Step 5 — Generate HTTP traffic
From Kali:
```bash
curl -I http://192.168.44.129/
```
In Wireshark, filter traffic involving Metasploitable:
```text
ip.addr == 192.168.44.129
```
Inspect the HTTP request and response.

### Step 6 — Inspect session cookies
Open the lab website in a browser. Open Developer Tools and navigate to **Storage/Application → Cookies**. Inspect the `HttpOnly`, `Secure`, and `SameSite` attributes. Use only a lab/test session; do not copy or reuse real session tokens.

### Step 7 — Understand session hijacking
Study how an exposed or stolen session identifier can allow impersonation. HTTPS encrypts traffic in transit; cookie attributes such as `Secure` and `HttpOnly` add protection.

### Step 8 — Stop capture and restore settings
Stop Wireshark and retain the capture as `experiment5.pcapng`. Disable forwarding:
```bash
sudo sysctl -w net.ipv4.ip_forward=0
cat /proc/sys/net/ipv4/ip_forward
```
Expected output from the second command: `0`.

## Observations
- ARP requests and replies were visible in Wireshark on the isolated lab interface.
- The neighbor table displayed IP-to-MAC mappings for devices on the lab network.
- HTTP traffic between Kali and Metasploitable was captured.
- The inspected cookie showed `HttpOnly=false`, `Secure=false`, and `SameSite=Lax`.

## Result
ARP traffic and HTTP communication were captured and analyzed, and browser session-cookie attributes were inspected.

## Conclusion
The experiment demonstrated ARP traffic observation, IP-to-MAC mapping inspection, HTTP traffic capture, and the importance of session-cookie security. The included screenshots and packet capture document these observations; they do not by themselves establish that a successful ARP-spoofing attack or session hijack occurred.

## Repository contents
- `README.md` — experiment report and procedure.
- `experiment5.pcapng` — Wireshark packet capture.
- `screenshots/` — six selected screenshots, excluding the screenshot with red TCP reset lines.

## Screenshot index
1. `screenshots/01-kali-ip-and-routing.png` — Kali IP addresses and routing.
2. `screenshots/02-ip-forwarding-enabled.png` — IPv4 forwarding enabled.
3. `screenshots/03-wireshark-arp-capture.png` — ARP packets in Wireshark.
4. `screenshots/04-arp-neighbor-table.png` — IP-to-MAC neighbor table.
5. `screenshots/05-http-traffic.png` — HTTP traffic involving Metasploitable.
6. `screenshots/06-session-cookie-inspection.png` — browser cookie attributes.
