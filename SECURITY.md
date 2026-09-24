# Security Policy

<div align="center">

# 🛡️ Security Policy

**Extracted — TryHackMe Walkthrough**

*A responsible disclosure and educational use policy for this cybersecurity portfolio repository.*

<img src="https://img.shields.io/badge/Security-Policy-success?style=for-the-badge&logo=shield" />
<img src="https://img.shields.io/badge/TryHackMe-Educational-red?style=for-the-badge&logo=tryhackme" />
<img src="https://img.shields.io/badge/Digital-Forensics-Portfolio-blue?style=for-the-badge" />

---

**Maintainer:** Anurag

Cybersecurity Portfolio • Digital Forensics • Network Security • SOC Learning

</div>

---

## 📖 Purpose

This repository documents a **TryHackMe Digital Forensics walkthrough** created for **educational, research, and portfolio purposes**.

The project demonstrates a forensic investigation involving:

- Network packet capture analysis.
- PowerShell malware analysis.
- TCP stream reconstruction.
- Base64 decoding.
- XOR deobfuscation.
- KeePass memory analysis.
- Credential recovery methodology.
- Incident response documentation.

The techniques are demonstrated **only within an authorized laboratory environment** provided by TryHackMe.

---

# 🎯 Scope

This repository is intended for:

- Cybersecurity students.
- SOC Analyst learners.
- Incident Response learners.
- Digital Forensics enthusiasts.
- Blue Team practitioners.
- Recruiters reviewing cybersecurity portfolio projects.

The documentation explains **how the investigation works**, **why each step is performed**, and **what evidence supports each finding**.

---

# 🔒 Supported Versions

| Version | Supported |
|----------|-----------|
| Latest `main` branch | ✅ Yes |
| GitHub Pages Documentation | ✅ Yes |
| Previous commits | ⚠️ No |
| Forks / Modified repositories | ❌ Not Supported |

Please use the latest version of this repository for documentation updates and fixes.

---

# 🚨 Reporting Security Issues

If you discover a security issue **within this repository** (broken documentation, accidental secret exposure, or sensitive information committed by mistake), please report it responsibly.

### Please report

- Accidentally exposed credentials.
- Sensitive artifacts committed by mistake.
- Personally identifiable information (PII).
- Incorrect security recommendations.
- Broken GitHub Pages deployment exposing unintended content.

### Please do NOT report

- TryHackMe room flags.
- Intended challenge solutions.
- Educational techniques demonstrated in the lab.
- Public tooling used during the investigation.

---

## 📬 Responsible Disclosure Process

1. Open a **private GitHub Security Advisory** if applicable.
2. Or create a **GitHub Issue** describing the documentation issue.
3. Include reproduction steps if the issue affects repository content.
4. Allow reasonable time for remediation before public discussion.

Please avoid publishing sensitive repository issues before they are fixed.

---

# ⚖️ Responsible Use

The techniques documented here include forensic and credential-analysis workflows.

These techniques **must only be used** against systems where you have **explicit authorization**.

### Authorized Use

- TryHackMe Labs.
- Hack The Box Labs.
- Capture The Flag environments.
- Personal lab environments.
- Corporate environments where written authorization exists.
- Educational demonstrations.

### Unauthorized Use

Do **not** use these techniques against:

- Production systems.
- Third-party networks.
- Personal devices you do not own.
- Corporate infrastructure without authorization.
- Password managers or memory dumps belonging to other users.

Unauthorized credential extraction or memory analysis may violate laws or organizational policies.

---

# 🧠 Educational Safety

This repository intentionally omits sensitive challenge artifacts.

## Public Redactions

The following information has been removed or masked:

| Sensitive Item | Status |
|----------------|--------|
| TryHackMe Flag | 🔒 Redacted |
| KeePass Master Password | 🔒 Redacted |
| Vault UUID | 🔒 Redacted |
| Private Credentials | 🔒 Removed |
| Personal Tokens / API Keys | 🔒 Removed |

Example:

```text
THM{FLAG_REDACTED}
```

No real credentials are published in this repository.

---

# 🛡️ Repository Security Practices

This repository follows several security practices to remain safe for public publication.

## Secrets Management

- No API keys.
- No authentication tokens.
- No passwords.
- No session cookies.
- No SSH private keys.
- No KeePass vault secrets.

## Artifact Handling

Artifacts shown in screenshots are educational evidence only.

The repository does **not** contain:

- Working malicious payloads.
- Live credential dumps.
- Original memory dumps.
- Original encrypted database containing secrets.

---

# 🔍 Security Review Checklist

Before publishing updates, verify:

- [x] No secrets committed.
- [x] Flags redacted.
- [x] Passwords masked.
- [x] UUIDs removed.
- [x] Screenshots reviewed.
- [x] No personal information exposed.
- [x] Documentation references only authorized labs.

---

# 📄 Third-Party Tools Used

This walkthrough references publicly available security tools for educational analysis.

| Tool | Purpose |
|------|---------|
| Wireshark | Network packet analysis |
| TShark | Packet extraction |
| Python 3 | Artifact reconstruction |
| KeePassXC CLI | Vault inspection |
| John the Ripper | Password validation |
| Community KeePass Memory Parser | Educational memory analysis |

All tools remain the property of their respective authors and projects.

---

# ⚠️ Legal Disclaimer

This repository is an educational write-up of a **TryHackMe Capture The Flag challenge**.

Nothing in this repository should be interpreted as permission to:

- Access unauthorized systems.
- Extract credentials from real users.
- Dump processes from production devices.
- Intercept network traffic without authorization.

Users are solely responsible for complying with local laws, organizational policies, and platform rules.

---

# 🧩 AI & Portfolio Disclosure

This repository is part of a personal cybersecurity portfolio.

Documentation may be enhanced with AI-assisted formatting, diagrams, summaries, and visual improvements while preserving the technical investigation methodology.

All practical investigation steps were reproduced inside an authorized lab environment.

---

# 🤝 Coordinated Disclosure Commitment

If sensitive information is accidentally published in this repository:

- It will be removed promptly.
- Repository history will be cleaned when appropriate.
- GitHub Security Advisories may be used when necessary.
- Updated documentation will be published after remediation.

---

<div align="center">

## 🔐 Security Through Responsible Learning

*Document. Analyze. Defend.*

**Extracted — TryHackMe Walkthrough**

Cybersecurity Portfolio by **Anurag**

</div>
