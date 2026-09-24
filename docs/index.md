---
layout: default
title: "Extracted | TryHackMe"
description: "PCAP-driven credential recovery and KeePass memory forensics"
---

<style>
.hero { padding: 28px 30px; margin-bottom: 28px; border: 1px solid #30363d; border-radius: 16px; background: linear-gradient(135deg,#0d1117,#161b22); }
.hero h1 { margin: 0 0 10px 0; font-size: 2.2rem; letter-spacing: -0.02em; }
.kicker { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; color:#8b949e; font-size:.9rem; }
.pill { display:inline-block; padding:5px 10px; margin:5px 5px 0 0; border:1px solid #30363d; border-radius:999px; font-size:.78rem; background:#0d1117; }
.callout { padding:16px 18px; border-left:4px solid #58a6ff; background:#0d1117; margin:18px 0; }
.figure { margin: 24px 0; }
.figure img { width:100%; max-width:1100px; border-radius:12px; border:1px solid #30363d; }
.caption { color:#8b949e; font-size:.88rem; margin-top:8px; }
.chain { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; padding:16px; background:#0d1117; border:1px solid #30363d; border-radius:12px; overflow:auto; }
</style>

<div class="hero">
  <div class="kicker">TRYHACKME / FORENSIC WALKTHROUGH</div>
  <h1>EXTRACTED</h1>
  <p>From packet capture to encrypted KeePass vault validation - an evidence-led investigation.</p>
  <span class="pill">Network Forensics</span>
  <span class="pill">PowerShell</span>
  <span class="pill">KeePass</span>
  <span class="pill">Memory Analysis</span>
  <span class="pill">PCAP</span>
</div>

<div class="callout">
<strong>Portfolio-safe publication.</strong> The original challenge flag, exact master password, and unique vault UUID are intentionally redacted.
</div>

## 01. Investigation map

<div class="chain">
PCAP -> HTTP PowerShell -> TCP/1337 + TCP/1338 -> Base64 -> XOR -> .dmp + .kdbx -> memory analysis -> credential validation -> vault
</div>

## 02. The first pivot: HTTP

The packet capture exposes a PowerShell script being delivered over HTTP. That request is the entry point for understanding how the data was collected and transmitted.

<div class="figure">
<img src="assets/img/01_wireshark_powershell_delivery.png" alt="Wireshark showing HTTP PowerShell traffic">
<div class="caption"><strong>Figure 01.</strong> HTTP traffic reveals the PowerShell delivery stage.</div>
</div>

### Key observation

The PowerShell stage identifies two exfiltration paths:

- **TCP/1337** - KeePass process dump
- **TCP/1338** - KeePass database

The script applies single-byte XOR and Base64 before transmission.

## 03. Artifact reconstruction

The network transformation is reversed during recovery:

```text
TCP stream
   -> Base64 decode
   -> XOR reversal
   -> original file
```

```bash
python3 recover.py stream_1338.raw Database1337.kdbx 0x42
python3 recover.py stream_1337.raw keepprocess.dmp 0x41
```

The resulting files can be validated with `file` and targeted string extraction.

## 04. Why memory changes the investigation

The `.kdbx` database is encrypted at rest. The process dump is valuable because secrets may exist in application memory while the password manager is operating.

<div class="figure">
<img src="assets/img/02_keepass_dump_extraction.png" alt="Terminal showing KeePass process dump analysis">
<div class="caption"><strong>Figure 02.</strong> Process-memory analysis produces a candidate secret representation.</div>
</div>

A candidate with an unknown character is handled as uncertainty, then tested against the actual vault rather than accepted blindly.

## 05. Validation

The final validation step is performed against the recovered `.kdbx`:

```bash
keepass2john Database1337.kdbx > db.hash
john --wordlist=candidates.txt db.hash
keepassxc-cli ls Database1337.kdbx
keepassxc-cli show Database1337.kdbx "You win!"
```

<div class="figure">
<img src="assets/img/03_keepass_vault_recovery.png" alt="KeePassXC CLI showing the recovered vault and target entry">
<div class="caption"><strong>Figure 03.</strong> KeePassXC confirms that the recovered credential opens the vault and the target entry is reachable. Sensitive values are redacted in this portfolio version.</div>
</div>

## 06. Toolset

| Tool | Purpose |
|---|---|
| Wireshark | Packet and stream inspection |
| tshark | Repeatable packet extraction |
| Python 3 | Decode/XOR recovery helper |
| xxd | Hex stream reconstruction |
| KeePass dump tooling | Memory artifact analysis |
| John the Ripper | Candidate verification |
| KeePassXC | Vault validation |

## 07. Lessons learned

### Evidence correlation beats guesswork

The room becomes straightforward only after the artifacts are connected in the right order. The HTTP stage explains the TCP streams; the TCP streams yield the files; the files explain the credential recovery step.

### Encrypted storage does not eliminate runtime exposure

A secure vault can still become an incident-response artifact when sensitive material is present in memory.

### Validation matters

Every intermediate result should be checked against the next artifact. This prevents corrupted streams, partial decoding, and false password candidates from being mistaken for success.

## 08. Repository contents

- `Documentation/Documentation.md` - full walkthrough
- `Resources/notes.md` - concise analyst reference
- `Screenshots/` - numbered evidence figures
- `docs/index.md` - GitHub Pages portfolio view

## 09. Flag

`THM{FLAG_REDACTED}`

> The original challenge answer is intentionally hidden in this public documentation.

---

**Author:** Anurag  
**Focus:** Network Forensics, PowerShell Analysis, KeePass Memory Forensics
