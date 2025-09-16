# Linux-Server-Hardening

## 🎯 Objective
Capture and analyze live network traffic to identify credentials or suspicious activity, then harden the server to prevent such leaks.

**Tools Used:**  
- Ubuntu Linux  
- UFW (firewall)  
- Fail2ban (brute-force protection)  
- SSH (secure remote access)  
- tcpdump / Wireshark (packet capture & analysis)  

---

## 📝 Before State
- UFW: `inactive`
- Fail2ban: not installed / not running
- FTP running (plaintext credentials possible)

---

## 🔬 Actions Performed
1. Installed and configured **fail2ban** (monitored SSH).  
2. Enabled and configured **UFW** (firewall rules).  
3. Installed and enabled **vsftpd** for testing plaintext FTP credentials.  
4. Captured traffic with **tcpdump** before and after hardening.  
5. Analyzed `.pcap` files in **Wireshark** to extract credentials.  
6. Disabled FTP + blocked port 21 with UFW after testing.  

---

## 📊 Findings
- **Before Hardening:** FTP login credentials (`USER`, `PASS`) were captured in plaintext.  
- **After Hardening:** FTP disabled & port 21 blocked → no credentials leaked.  

---

## 📂 Project Files
- `report/` → Detailed Word/PDF report  
- `evidence/` → Captured traffic, extracted credentials  
- `screenshots/` → Proof of firewall/fail2ban/tcpdump in action  
- `applied_commands.sh` → All commands executed  
