# Contributing to Extracted – TryHackMe Walkthrough

<div align="center">

# 🤝 Contributing Guide

**Extracted — TryHackMe Walkthrough**

*A professional contribution guide for this Digital Forensics & Network Security portfolio repository.*

<img src="https://img.shields.io/badge/Contributions-Welcome-success?style=for-the-badge&logo=github" />
<img src="https://img.shields.io/badge/TryHackMe-Educational-red?style=for-the-badge&logo=tryhackme" />
<img src="https://img.shields.io/badge/Open%20Source-Cybersecurity-blue?style=for-the-badge&logo=opensourceinitiative" />

---

**Maintainer:** Anurag

Cybersecurity Portfolio • Digital Forensics • Incident Response • Network Security

</div>

---

## 👋 Welcome

Thank you for your interest in contributing to **Extracted – TryHackMe Walkthrough**.

This repository is part of a professional cybersecurity portfolio documenting a **TryHackMe Digital Forensics challenge**. Contributions are welcome if they improve the educational quality, technical accuracy, reproducibility, or documentation of the project.

The goal is to keep this repository **accurate, ethical, beginner-friendly, and portfolio-quality**.

---

# 🎯 Project Goals

This repository aims to teach and document:

- Network Packet Capture Analysis
- Wireshark Investigation Workflow
- TCP Stream Reconstruction
- PowerShell Payload Analysis
- Base64 & XOR Artifact Recovery
- KeePass Memory Analysis
- Credential Validation Methodology
- Incident Response Documentation

Contributions should support these learning objectives.

---

# ✅ Ways You Can Contribute

We welcome improvements in the following areas.

## 📚 Documentation

Improve or expand:

- Technical explanations.
- Grammar and readability.
- Investigation flow.
- MITRE ATT&CK mapping.
- Detection engineering notes.
- Defensive recommendations.
- Incident response insights.

Examples:

- Better explanation of TCP stream reconstruction.
- Additional Wireshark filters.
- More defensive analysis.
- Better PowerShell explanation.

---

## 🛠️ Educational Improvements

Suggestions may include:

- Better Python helper scripts.
- Improved forensic workflow diagrams.
- Better screenshots.
- Jekyll documentation enhancements.
- GitHub Pages improvements.
- Dark/light theme improvements.

---

## 🎨 GitHub Pages UI

You may contribute improvements to:

- `docs/index.md`
- `docs/assets/css/custom.scss`
- Layout styling.
- Responsive design.
- Navigation.
- Code block styling.
- Timeline sections.

Please keep the overall **professional cybersecurity aesthetic** consistent.

---

## 🔐 Security Improvements

Security-focused contributions include:

- Better defensive mitigations.
- Detection opportunities.
- Blue Team recommendations.
- Sigma rule ideas.
- YARA discussion.
- MITRE ATT&CK references.
- Incident response improvements.

---

# 🚫 What Should NOT Be Contributed

Please **do not** submit pull requests that include:

## Sensitive Information

- TryHackMe flags.
- Real KeePass master passwords.
- API keys.
- Authentication tokens.
- Personal credentials.
- Session cookies.
- Private memory dumps.
- Original `.kdbx` databases containing secrets.

## Malicious Content

Do not upload:

- Weaponized malware.
- Exploit payloads targeting real systems.
- Live phishing kits.
- Credential stealers.
- Unauthorized offensive tooling.

This repository is educational, not offensive tooling.

---

# 📁 Repository Structure

```text
Extracted-TryHackMe-Walkthrough/
│
├── README.md
├── SECURITY.md
├── CONTRIBUTING.md
│
├── Documentation/
│   ├── Documentation.md
│   └── Extracted_TryHackMe_Documentation.docx
│
├── Resources/
│   └── notes.md
│
├── Screenshots/
│   ├── 01_wireshark_powershell_delivery.png
│   ├── 02_keepass_dump_extraction.png
│   ├── 03_keepass_vault_recovery.png
│   └── ...
│
├── docs/
│   ├── index.md
│   ├── _config.yml
│   └── assets/
│       ├── css/
│       └── img/
│
└── .github/
    └── workflows/
```

Please keep filenames and folder structure consistent.

---

# 📸 Screenshot Guidelines

Screenshots should be:

- High resolution.
- Dark theme preferred.
- Cropped to relevant evidence.
- Free from personal information.
- Free from desktop notifications.
- Numbered sequentially.

### Naming Convention

| Screenshot | Filename |
|------------|----------|
| Wireshark Evidence | `01_wireshark_powershell_delivery.png` |
| KeePass Dump Analysis | `02_keepass_dump_extraction.png` |
| KeePass Vault Recovery | `03_keepass_vault_recovery.png` |
| Additional Evidence | `04_*.png` |

Do not upload duplicate or low-quality screenshots.

---

# ✍️ Documentation Style Guide

Use clean Markdown.

## Headings

```md
# Major Section

## Investigation Phase

### Technical Details
```

## Code Blocks

Always specify the language.

````md
```bash
tshark -r traffic.pcapng \
-Y "tcp.port == 1338"
```
