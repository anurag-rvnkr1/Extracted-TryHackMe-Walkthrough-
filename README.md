# Extracted - TryHackMe Walkthrough

[![TryHackMe](https://img.shields.io/badge/Platform-TryHackMe-red?logo=tryhackme)](https://tryhackme.com/)
[![Focus](https://img.shields.io/badge/Focus-Network%20Forensics-blue)](#)
[![Docs](https://img.shields.io/badge/Docs-GitHub%20Pages-222222?logo=githubpages)](./docs/index.md)
[![Status](https://img.shields.io/badge/Write--up-Complete-success)](#)

> **A forensic-style walkthrough of a PCAP-driven KeePass extraction challenge.**

This repository documents the full investigation path from packet capture triage to recovery and validation of an encrypted KeePass vault.

### Attack / investigation chain

```text
PCAP
  |
  +--> HTTP PowerShell delivery
  |
  +--> TCP/1337 ----> process dump
  |
  +--> TCP/1338 ----> KeePass database
             |
             v
       Base64 + XOR reversal
             |
             v
       KeePass artifacts
             |
             v
       memory analysis
             |
             v
       candidate master key
             |
             v
       vault validation
```

### What this project demonstrates

- Wireshark and TCP stream analysis
- PowerShell payload inspection
- Base64 + XOR artifact recovery
- Windows minidump analysis
- KeePass credential recovery methodology
- John the Ripper verification
- KeePassXC command-line validation
- Evidence-driven incident analysis

### Documentation

- [Full technical documentation](Documentation/Documentation.md)
- [Analyst notes / command reference](Resources/notes.md)
- [Portfolio-ready GitHub Pages](docs/index.md)

### Flag policy

The final challenge flag is intentionally redacted in this public repository. The write-up preserves the methodology, evidence chain, tooling, and validation process without publishing the direct answer.

### Ethical use

This material is for authorized TryHackMe/lab environments, defensive research, education, and portfolio documentation. Do not apply the techniques to systems or credentials without explicit authorization.

---
**Author:** Anurag  
**Platform:** TryHackMe  
**Topic:** Network Forensics / PowerShell / KeePass Memory Analysis
