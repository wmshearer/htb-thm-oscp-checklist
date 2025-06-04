# 🎯 Advanced Penetration Testing Checklist for HTB, THM & OSCP

A **comprehensive penetration testing checklist and methodology** for **Hack The Box (HTB)**, **TryHackMe (THM)**, and **OSCP-like labs**. This guide is designed for cybersecurity learners, ethical hackers, CTF enthusiasts, and OSCP aspirants seeking a **structured, repeatable approach** to **reconnaissance, enumeration, exploitation, privilege escalation, post-exploitation, lateral movement, and objective hunting**.  
**Keywords:** penetration testing, CTF, Hack The Box, TryHackMe, OSCP, ethical hacking, cybersecurity, Kali Linux, methodology, checklist.

---

## 🚀 Features

- Step-by-step methodology for penetration testing labs and CTFs
- Covers all phases: Reconnaissance, Vulnerability Assessment, Exploitation, Post-Exploitation, Privilege Escalation, Lateral Movement, and Objective Hunting
- Quick access to essential tools, commands, and troubleshooting tips
- Optimized for Kali Linux, Parrot OS, and other popular pentesting distributions
- Suitable for OSCP, HTB, THM, and similar platforms

---

## 🏁 Getting Started

1. **Clone this repository** to your local machine:
   ```bash
   git clone https://github.com/yourusername/htb-thm-oscp-checklist.git
   cd htb-thm-oscp-checklist
   ```
2. **Review the checklist** and customize it for your workflow.
3. **Follow each phase** as you progress through your penetration testing engagement or CTF challenge.

---

## 🛠️ Pre-Engagement Setup

Before starting any penetration test or CTF, ensure your environment is ready:

### Environment Preparation
- [ ] **VPN Connection**: Verify HTB/THM VPN connectivity  
  ```bash
  ip a | grep tun0  # Check for VPN interface
  ping -c 1 8.8.8.8  # Verify internet connectivity
  ```
- [ ] **Note-Taking Setup**: Initialize workspace (Obsidian, CherryTree, Notion)
- [ ] **Screenshot Tools**: Configure Flameshot/Greenshot with hotkeys
- [ ] **Terminal Setup**: Multiple terminals/tmux sessions ready
- [ ] **Essential Wordlists**: Verify SecLists installation and updates
- [ ] **Proxy Setup**: Configure Burp Suite/OWASP ZAP for web testing
- [ ] **Docker/VM**: Ensure isolated testing environment if needed

### Initial Information Gathering
- [ ] **Target Information**: Record machine details, IP, difficulty, OS hints
- [ ] **Platform Tags/Hints**: Note all provided hints and tags carefully
- [ ] **Research Phase**: Quick Google search for machine name (if not exam)
- [ ] **Time Tracking**: Set up timer for phase management

---

## 📋 Methodology Phases

After completing initial configuration, follow this systematic approach:

### Phase 1: 🔍 [Reconnaissance and Enumeration](./recon_and_enum/README.md)
### Phase 2: 🛡️ [Vulnerability Assessment](./vulnerability_assessment.md)
### Phase 3: ⚡ [Exploitation](./exploitation.md)
### Phase 4: 📊 [Post-Exploitation](./post_exploitation.md)
### Phase 5: 📈 [Privilege Escalation](./privilege_escalation.md)
### Phase 6: 🔄 [Lateral Movement and Persistence](./lateral_movement_and_persistence.md)
### Phase 7: 🎯 [Objective Hunting](./objective_hunting.md)

---

## 🛠️ Resources and References

- 📚 [Toolset and Techniques](./quick_access/toolset_and_techniques.md)
- ⚡ [Quick Commands](./quick_access/quick_commands.md)
- 🔧 [Troubleshooting Tips](./quick_access/troubleshooting_tips.md)
- 📝 [Conclusion](./quick_access/conclusion.md)

---

*This checklist is maintained by [cyb3ritic](https://github.com/cyb3ritic). For more cybersecurity resources, visit [my GitHub](https://github.com/cyb3ritic).*

**Tags:** penetration-testing, oscp, hackthebox, tryhackme, ctf, ethical-hacking, kali-linux, methodology, checklist

**Version**: 2.1  
**Last Updated**: June 2025  
**Environment**: Kali Linux / HTB/THM Platforms
















