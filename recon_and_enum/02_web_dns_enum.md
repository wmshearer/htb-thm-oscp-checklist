# 🌍 Web & DNS Enumeration

## 🕸️ Web Services

### Manual Recon
- [ ] Browse all pages, forms, JS
- [ ] Check robots.txt, sitemap.xml, changelogs, .git/.svn
- [ ] Identify tech stack:  
  `whatweb http://<target>`  
  `curl -I http://<target>`

### Directory & File Discovery
- [ ] **Gobuster (Common)**:
  ```bash
  gobuster dir -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,txt,html,bak,old,zip -o gobuster_common.txt
  ```
- [ ] **Gobuster (Comprehensive)**:
  ```bash
  gobuster dir -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,txt,html,asp,aspx,jsp -o gobuster_medium.txt
  ```
- [ ] **FFuF (Faster Alternative)**:
  ```bash
  ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://<target>/FUZZ -e .php,.txt,.html,.bak -mc 200,301,302,403 -o ffuf_results.json
  ```
- [ ] **Dirsearch**:
  ```bash
  dirsearch.py -u http://<target> -e php,txt,html,js,bak -x 404,403
  ```
- [ ] **Feroxbuster** (Recursive):
  ```bash
  feroxbuster -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,txt,html -r
  ```

### Advanced Web Enum
- [ ] **Parameter Discovery**:
  ```bash
  ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://<target>/page?FUZZ=test -mc 200
  ```
- [ ] **Virtual Host Discovery**:
  ```bash
  gobuster vhost -u http://<target> -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
  ```
- [ ] **API Endpoint Discovery**:
  ```bash
  gobuster dir -u http://<target> -w /usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt
  ```


## DNS (53)

### Basic DNS Enumeration
- [ ] **Zone Transfer Attempts**:
  ```bash
  dig axfr @<target> <domain>
  dnsrecon -d <domain> -t axfr
  ```
- [ ] **DNS Brute-forcing**:
  ```bash
  dnsrecon -d <domain> -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t brt
  ```

### Advanced DNS Testing
- [ ] **DNS Cache Snooping**:
  ```bash
  nmap --script dns-cache-snoop <target>
  ```
- [ ] **DNS Recursion Testing**:
  ```bash
  nmap --script dns-recursion <target>
  ```

---

## 🧩 Subdomain Enumeration

- [ ] **Subdomain brute-force (ffuf)**:  
  ```bash
  ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.<domain>" -u http://<target> -fs <size>
  ```
- [ ] **Gobuster DNS**:  
  ```bash
  gobuster dns -d <domain> -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
  ```
- [ ] **Amass (if available)**:  
  ```bash
  amass enum -d <domain>
  ```

---