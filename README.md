# 🎯 Advanced HTB/THM Penetration Testing Checklist

## 🚀 Pre-Engagement Setup

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

### Phase 1: 🔍 [Reconnaissance and Enumeration](./recon_and%20_enum/recon_and_enum.md)

### Phase 2: 🛡️ [Vulnerability Assessment](./vulnerability_assessment.md)

### Phase 3: ⚡ [Exploitation](./exploitation.md)

### Phase 4: 📊 [Post-Exploitation](./post_exploitation.md)

### Phase 5: 📈 [Privilege Escalation](./privilege_escalation.md)

### Phase 6: 🔄 [Lateral Movement and Persistence](./lateral_movement_and_persistence.md)

### Phase 7: 🎯 [Objective Hunting](./objective_hunting.md)

---

## 🛠️ Resources and References

### Quick Access
- 📚 [Toolset and Techniques](./quick_access/toolset_and_techniques.md) - Complete arsenal of tools and methods
- ⚡ [Quick Commands](./quick_access/quick_commands.md) - Essential one-liners and shortcuts
- 🔧 [Troubleshooting Tips](./quick_access/troubleshooting_tips.md) - Common issues and solutions
- 📝 [Conclusion](./quick_access/conclusion.md) - Reporting and documentation guidelines

---

*Remember: The goal is not just to capture flags, but to develop a systematic, repeatable methodology that works under pressure and scales to real-world engagements.*

---

**Version**: 2.1 
**Last Updated**: June 2025  
**Maintainer**: cyb3ritic  
**Environment**: Kali Linux / HTB/THM Platforms
















