# 🌐 Network Discovery & Scanning

## 🔎 Initial Discovery
- [ ] Ping sweep:  
  `fping -a -g <subnet>/24`
- [ ] ARP scan (local):  
  `arp-scan -l`

## 🚀 Quick Nmap Scans
- [ ] Fast SYN scan:  
  `nmap -sS -T4 -F <target> -oN nmap_fast.txt`
- [ ] Top 1000 ports:  
  `nmap --top-ports 1000 -sV <target> -oN nmap_top1000.txt`
- [ ] Default scripts:  
  `nmap -sC -sV <target> -oN nmap_initial.txt`

## 🏁 Full Port Scanning
- [ ] All TCP ports:  
  `nmap -p- --min-rate 10000 <target> -oN nmap_alltcp.txt`
- [ ] All TCP with scripts:  
  `nmap -p- -sC -sV --min-rate 5000 <target> -oN nmap_full.txt`
- [ ] Top 100 UDP:  
  `nmap -sU --top-ports 100 <target> -oN nmap_udp.txt`

## 🧠 Advanced Nmap
- [ ] Aggressive scan:  
  `nmap -A -T4 <target> -oN nmap_aggressive.txt`
- [ ] OS detection:  
  `nmap -O <target> -oN nmap_os.txt`
- [ ] Firewall evasion:  
  `nmap -f -D RND:10 <target>`
  `nmap -sF <target>`
  `nmap -sX <target>`

## ⚡ Masscan (super fast)
- [ ] All ports:  
  `masscan -p1-65535,U:1-65535 <target> --rate=10000`

---