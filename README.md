# Linux-Server-Hardening

## 🎯 Objective
Capture and analyze live network traffic to identify credentials or suspicious activity, then harden the server to prevent such leaks.

**Tools Used:**
- Ubuntu Linux
- UFW (firewall)
- Fail2ban (brute-force protection)
- SSH (secure remote access)
- tcpdump / Wireshark (packet capture & analysis)
- vsftpd, Apache, Telnet (for generating test traffic)

---

## 📝 Before State
- Firewall (UFW): **inactive**
- Fail2ban: **not installed**
- SSH: **Root login allowed, password authentication enabled**
- Open Ports: **22 (SSH), 80 (HTTP), 3306 (MySQL – exposed)**
- Captured FTP/Telnet/HTTP traffic showed **plaintext usernames and passwords**

---

## 🔬 Actions Performed
1. Installed **tcpdump** and **tshark** for packet capture.
2. Generated **FTP, Telnet, and HTTP test traffic** with credentials.
3. Captured packets and analyzed in **Wireshark**:
   - FTP → `USER` and `PASS` visible in plaintext.
   - Telnet → credentials visible when following TCP stream.
   - HTTP → `Authorization: Basic` headers revealed credentials (base64-decoded).
4. Installed and configured **UFW** (firewall rules).
5. Installed and configured **Fail2ban** to protect SSH.
6. Hardened **SSH**:
   - Disabled root login.
   - Disabled password authentication.
   - Enforced key-based login.
7. Disabled plaintext services (FTP, Telnet) and blocked ports in UFW.
8. Captured traffic again → **no plaintext credentials observed**.

---

## 📊 Findings
- **Before Hardening:** Plaintext credentials (FTP, Telnet, HTTP Basic Auth) were captured.
- **After Hardening:** Services disabled/blocked; only secure SSH allowed.  
  No plaintext credentials visible in packet captures.

---

## 🚀 How to Reproduce
1. Install tools:
   ```bash
   sudo apt update
   sudo apt install -y tcpdump tshark ufw fail2ban vsftpd apache2 telnetd ftp curl telnet
   ```
2. Run services (FTP, Telnet, HTTP) and generate test traffic.
3. Capture traffic:
   ```bash
   sudo tcpdump -i ens33 -s 0 -w capture_before.pcap      'tcp port 21 or tcp port 23 or tcp port 80'
   ```
4. Analyze in Wireshark with filters:
   - `ftp.request.command == "USER" || ftp.request.command == "PASS"`
   - `telnet`
   - `http.authorization`
5. Apply hardening (disable FTP/Telnet, configure UFW + Fail2ban, SSH key auth).
6. Capture again → confirm no credentials in plaintext.

---

## 📌 Author
**Subhash Chandra Bose Vallapu**  
Cybersecurity Enthusiast | Linux Hardening & Network Security
