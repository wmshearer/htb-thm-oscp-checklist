# 🛠️ Additional Service-Specific Enumeration

## HTTP Proxy (8080, 3128, 8888)

### Basic Proxy Enumeration
- [ ] **Check for Open Proxy**:
  ```bash
  nmap -p 8080,3128,8888 --script http-open-proxy <target>
  ```
- [ ] **Test Proxy Functionality**:
  ```bash
  curl --proxy http://<target>:8080 http://httpbin.org/ip
  ```
- [ ] **CONNECT Method Test**:
  ```bash
  # Check if CONNECT method allows tunneling
  nc <target> 8080
  CONNECT google.com:80 HTTP/1.1
  ```

## RMI/JRMP (1099, 1098)

### Basic RMI Enumeration
- [ ] **Nmap RMI Scripts**:
  ```bash
  nmap -p 1099 --script rmi-dumpregistry,rmi-vuln-classloader <target>
  ```
- [ ] **RMI Registry Enumeration**:
  ```bash
  # Use ysoserial or RMIScout for exploitation
  ```

## X11 (6000-6063)

### Basic X11 Enumeration
- [ ] **Check for Open X11**:
  ```bash
  nmap -p 6000-6063 --script x11-access <target>
  ```
- [ ] **X11 Screenshot**:
  ```bash
  # If X11 is open
  xdpyinfo -display <target>:0
  xwininfo -root -tree -display <target>:0
  ```
- [ ] **X11 Keylogger**:
  ```bash
  xev -display <target>:0
  ```

## MQTT (1883, 8883)

### Basic MQTT Enumeration
- [ ] **Nmap MQTT Scripts**:
  ```bash
  nmap -p 1883,8883 --script mqtt-subscribe <target>
  ```
- [ ] **MQTT Client Connect**:
  ```bash
  mosquitto_sub -h <target> -t '#' -v
  mosquitto_pub -h <target> -t 'test' -m 'hello'
  ```

## Elasticsearch (9200)

### Basic Elasticsearch Enumeration
- [ ] **Cluster Info**:
  ```bash
  curl http://<target>:9200/
  curl http://<target>:9200/_cluster/health
  ```
- [ ] **List Indices**:
  ```bash
  curl http://<target>:9200/_cat/indices
  ```

## Java RMI Registry (1099)

### Advanced RMI Exploitation
- [ ] **ysoserial Integration**:
  ```bash
  # Download ysoserial.jar
  java -cp ysoserial.jar ysoserial.exploit.RMIRegistryExploit <target> 1099 CommonsCollections6 'command'
  ```

## Database Services
- [ ] **MySQL (3306)**:
  ```bash
  nmap --script mysql-info,mysql-audit,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <target>
  ```
- [ ] **PostgreSQL (5432)**:
  ```bash
  nmap --script pgsql-brute <target>
  ```
- [ ] **MSSQL (1433)**:
  ```bash
  nmap --script ms-sql-info,ms-sql-config,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=password <target>
  ```
- [ ] **CouchDB (5984)**:
  ```bash
  curl http://<target>:5984/
  curl http://<target>:5984/_all_dbs
  curl http://<target>:5984/<dbname>/_all_docs
  ```

- [ ] **MongoDB (27017)**:
  ```bash
  nmap -p 27017 --script mongodb-info <target>
  mongo <target>:27017

  # In mongo shell
  show dbs
  use <database>
  show collections
  ```

## Mail Services
- [ ] **SMTP (25, 587)**:
  ```bash
  smtp-user-enum -M VRFY -U users.txt -t <target>
  nmap --script smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 <target>
  ```
- [ ] **POP3 (110)**:
  ```bash
  nmap --script pop3-capabilities,pop3-ntlm-info <target>
  ```
- [ ] **IMAP (143)**:
  ```bash
  nmap --script imap-capabilities,imap-ntlm-info <target>
  ```

### RDP (3389)
- [ ] **RDP Information**:
  ```bash
  nmap --script rdp-enum-encryption,rdp-ntlm-info,rdp-vuln-ms12-020 <target>
  ```
- [ ] **RDP Screenshots** (if tools available):
  ```bash
  rdesktop <target>
  ```

### LDAP (389, 636)
- [ ] **LDAP Enumeration**:
  ```bash
  nmap --script ldap-rootdse,ldap-search <target>
  ldapsearch -x -h <target> -s base namingcontexts
  ```

---