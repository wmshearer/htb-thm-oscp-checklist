## 🚨 Advanced Troubleshooting & Tips

### Common Issues & Solutions

#### Shell Issues
- [ ] **Shell Dies Quickly**:
  - Check for shell timeout settings
  - Use shell stabilization techniques
  - Consider using encrypted shells (HTTPS reverse shells)

- [ ] **Special Characters Not Working**:
  - URL encode payloads
  - Try different encoding methods
  - Use alternative payload formats

#### Network Issues
- [ ] **Firewall Blocking**:
  - Try different ports (80, 443, 53, 1337)
  - Use HTTP/HTTPS tunneling
  - Fragment packets or use decoys

- [ ] **NAT/Routing Issues**:
  - Verify VPN connectivity
  - Check routing tables
  - Use correct network interfaces

#### Privilege Escalation Failures
- [ ] **SUID Not Working**:
  - Check for `nosuid` mount options
  - Verify binary permissions
  - Look for alternative escalation paths

- [ ] **Sudo Restrictions**:
  - Check for command restrictions
  - Look for sudo version vulnerabilities
  - Try alternative privilege escalation methods

### Pro Tips for Easy/Medium Machines

#### Time Management
- [ ] **Enumeration Focus**: Spend 60% of time on enumeration
- [ ] **Tool Rotation**: If one tool doesn't work, try alternatives
- [ ] **Note Everything**: Keep detailed notes of all findings
- [ ] **Regular Breaks**: Take breaks to maintain focus

#### Strategic Approach
- [ ] **Low-Hanging Fruit**: Always check for default credentials first
- [ ] **Version Checking**: Always identify software versions
- [ ] **Multiple Vectors**: Try different attack vectors simultaneously
- [ ] **Documentation**: Document working exploits for future reference

#### Common Oversights
- [ ] **Hidden Directories**: Don't forget about .git, .svn, .DS_Store
- [ ] **Alternative Ports**: Check for services on non-standard ports
- [ ] **File Extensions**: Try multiple extensions in directory brute-forcing
- [ ] **User-Agent Strings**: Some applications filter based on User-Agent

### Emergency Procedures

#### When Stuck
1. **Reset the Machine**: Sometimes machines get corrupted
2. **Re-read All Output**: Look for missed clues in tool output
3. **Try Different Wordlists**: Use larger or specialized wordlists
4. **Check Forums**: Look for hints (without spoilers) on platforms
5. **Take a Break**: Sometimes a fresh perspective helps

#### Clean Up
- [ ] **Remove Uploaded Files**: Clean up any files uploaded during testing
- [ ] **Kill Background Processes**: Stop any long-running scans
- [ ] **Document Lessons Learned**: Note what worked and what didn't

---