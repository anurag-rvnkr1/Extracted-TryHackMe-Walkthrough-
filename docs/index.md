---
layout: default
title: "Extracted | TryHackMe Digital Forensics Walkthrough"
description: "Professional DFIR portfolio documenting packet capture analysis, PowerShell malware investigation, TCP stream reconstruction, KeePass memory forensics, and credential recovery."
permalink: /
---

<link rel="stylesheet" href="assets/css/custom.css">

<div align="center">

# 🔍 EXTRACTED

### Digital Forensics • Incident Response • Network Analysis • Memory Forensics

<img src="https://img.shields.io/badge/TryHackMe-Digital%20Forensics-red?style=for-the-badge&logo=tryhackme"/>

<img src="https://img.shields.io/badge/Investigation-PCAP%20Analysis-blue?style=for-the-badge"/>

<img src="https://img.shields.io/badge/Memory-KeePass%20Forensics-success?style=for-the-badge"/>

<img src="https://img.shields.io/badge/Portfolio-DFIR%20Report-111111?style=for-the-badge&logo=github"/>

---

*A complete forensic investigation documenting the recovery of an encrypted KeePass vault from captured network traffic through malware analysis, TCP stream reconstruction, memory forensics, and credential validation.*

</div>

---

# 🛡️ Incident Investigation Dashboard

<table>
<tr>
<td align="center" width="25%">

## 🧩 Category

Digital Forensics

Network Analysis

</td>

<td align="center" width="25%">

## ⚡ Difficulty

Medium

TryHackMe

</td>

<td align="center" width="25%">

## 🎯 Focus

PowerShell Malware

KeePass Memory Analysis

</td>

<td align="center" width="25%">

## 📅 Status

Completed

Portfolio Ready

</td>
</tr>
</table>

---

# 📌 Investigation Snapshot

| Investigation Metric | Value |
|----------------------|-------|
| **Evidence Source** | Network Packet Capture (`.pcapng`) |
| **Primary Malware** | PowerShell Script |
| **Recovered Artifacts** | KeePass Database + Process Dump |
| **Network Streams Investigated** | HTTP + TCP/1337 + TCP/1338 |
| **Memory Artifact** | Windows MiniDump |
| **Credential Store** | KeePass (`.kdbx`) |
| **Final Objective** | Vault Authentication *(Redacted)* |
| **Report Type** | Digital Forensics & Incident Response |

---

# 🧠 Executive Summary

> **Extracted** is a realistic **Digital Forensics and Incident Response (DFIR)** investigation where the analyst reconstructs attacker behavior from captured network evidence instead of exploiting a vulnerable application.

The challenge simulates a credential theft campaign targeting a Windows workstation running **KeePass Password Manager**. A malicious PowerShell payload is delivered over HTTP, creates a process memory dump, steals an encrypted KeePass vault, obfuscates both artifacts using XOR and Base64, and exfiltrates them across separate TCP channels.

This investigation demonstrates how defenders correlate **network evidence**, **malware behavior**, **memory artifacts**, and **encrypted credential stores** to recover attacker activity inside an authorized TryHackMe laboratory.

---

# 🎯 Investigation Objectives

<table>
<tr>
<td width="50%">

### Network Forensics

- HTTP Traffic Analysis
- Packet Capture Investigation
- TCP Stream Reconstruction
- Payload Extraction
- Base64 Recovery

</td>

<td width="50%">

### Memory & Credential Forensics

- PowerShell Reverse Engineering
- KeePass Memory Dump Analysis
- Password Candidate Recovery
- Vault Authentication
- Digital Evidence Validation

</td>
</tr>
</table>

---

# ⚔️ Attack Chain Overview

The complete investigation follows a real-world credential theft workflow.

## Evidence Correlation Flow

```text
                    Packet Capture (.pcapng)
                              │
                              ▼
                HTTP PowerShell Payload Delivery
                              │
                              ▼
                 PowerShell Malware Investigation
                              │
        ┌─────────────────────┴────────────────────┐
        ▼                                          ▼
 TCP Stream — Port 1337                     TCP Stream — Port 1338
 KeePass Process Dump                       KeePass Database Vault
        │                                          │
        ▼                                          ▼
     Base64 Decode                             Base64 Decode
        │                                          │
        ▼                                          ▼
      XOR 0x41                                  XOR 0x42
        │                                          │
        ▼                                          ▼
  Windows MiniDump                          KeePass Database (.kdbx)
                └────────────────┬────────────────┘
                                 ▼
                     Memory Credential Analysis
                                 ▼
                  Candidate Master Password Recovery
                                 ▼
                    KeePass Vault Authentication
                                 ▼
                     Flag Validation (Redacted)
```

---

# 🧬 DFIR Attack Chain Visualization

<svg viewBox="0 0 760 700" xmlns="http://www.w3.org/2000/svg">

<style>
.node{fill:#0d1117;stroke:#22c55e;stroke-width:2;}
.line{stroke:#22c55e;stroke-width:2;marker-end:url(#a);}
.txt{fill:#c9d1d9;font-size:14px;font-family:monospace;}
.small{fill:#8b949e;font-size:12px;font-family:monospace;}
.title{fill:#22c55e;font-size:16px;font-weight:bold;font-family:monospace;}
</style>

<defs>
<marker id="a" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto">
<polygon points="0 0,8 4,0 8" fill="#22c55e"/>
</marker>
</defs>

<rect x="220" y="20" width="320" height="55" rx="10" class="node"/>
<text x="280" y="50" class="title">PACKET CAPTURE (.PCAPNG)</text>

<line x1="380" y1="75" x2="380" y2="105" class="line"/>

<rect x="180" y="105" width="400" height="60" rx="10" class="node"/>
<text x="240" y="140" class="txt">HTTP POWERSHELL PAYLOAD DELIVERY</text>

<line x1="380" y1="165" x2="380" y2="200" class="line"/>

<rect x="170" y="200" width="420" height="60" rx="10" class="node"/>
<text x="230" y="235" class="txt">POWERSHELL MALWARE ANALYSIS</text>

<line x1="380" y1="260" x2="250" y2="300" class="line"/>
<line x1="380" y1="260" x2="510" y2="300" class="line"/>

<rect x="30" y="300" width="260" height="80" rx="10" class="node"/>
<text x="75" y="330" class="txt">TCP/1337</text>
<text x="60" y="355" class="small">KeePass Process Dump</text>

<rect x="470" y="300" width="260" height="80" rx="10" class="node"/>
<text x="520" y="330" class="txt">TCP/1338</text>
<text x="505" y="355" class="small">KeePass Database</text>

<line x1="160" y1="380" x2="160" y2="430" class="line"/>
<line x1="600" y1="380" x2="600" y2="430" class="line"/>

<rect x="40" y="430" width="240" height="60" rx="10" class="node"/>
<text x="70" y="465" class="small">Base64 Decode → XOR 0x41</text>

<rect x="480" y="430" width="240" height="60" rx="10" class="node"/>
<text x="510" y="465" class="small">Base64 Decode → XOR 0x42</text>

<line x1="160" y1="490" x2="310" y2="545" class="line"/>
<line x1="600" y1="490" x2="450" y2="545" class="line"/>

<rect x="210" y="545" width="340" height="70" rx="10" class="node"/>
<text x="250" y="575" class="txt">KEEPASS MEMORY FORENSICS</text>
<text x="255" y="598" class="small">Credential Recovery & Validation</text>

<line x1="380" y1="615" x2="380" y2="660" class="line"/>

<rect x="180" y="660" width="400" height="50" rx="10" class="node"/>
<text x="205" y="690" class="txt">KEEPASS VAULT AUTHENTICATION (REDACTED)</text>

</svg>

---

# 🧾 Investigation Timeline

<table>
<tr>
<th width="18%">Phase</th>
<th width="28%">Investigation Area</th>
<th>Outcome</th>
</tr>

<tr>
<td>01</td>
<td>Packet Capture Analysis</td>
<td>Suspicious HTTP PowerShell request identified.</td>
</tr>

<tr>
<td>02</td>
<td>PowerShell Reverse Engineering</td>
<td>Exfiltration methodology discovered.</td>
</tr>

<tr>
<td>03</td>
<td>TCP Stream Reconstruction</td>
<td>Encoded artifacts recovered.</td>
</tr>

<tr>
<td>04</td>
<td>Base64 + XOR Recovery</td>
<td>KeePass vault and memory dump reconstructed.</td>
</tr>

<tr>
<td>05</td>
<td>Memory Forensics</td>
<td>Candidate master password extracted.</td>
</tr>

<tr>
<td>06</td>
<td>Credential Validation</td>
<td>KeePass vault authenticated successfully.</td>
</tr>

<tr>
<td>07</td>
<td>Incident Findings</td>
<td>Challenge objective validated without exposing secrets.</td>
</tr>

</table>

---

# 📸 Evidence Collection Gallery

The investigation is evidence-driven. Each screenshot represents a critical milestone during the forensic workflow.

---

## Phase 1 — Network Evidence

<img src="assets/01_wireshark_powershell_delivery.png" width="100%">

**Figure 01 — Wireshark reveals the malicious HTTP PowerShell payload delivered to the Windows host.**

**Evidence Collected**

- HTTP GET request.
- Windows PowerShell User-Agent.
- Malicious `.ps1` payload.
- Internal attacker infrastructure.

---

## Phase 2 — Memory Artifact Recovery

<img src="assets/02_keepass_dump_extraction.png" width="100%">

**Figure 02 — KeePass process dump analyzed to recover candidate credential material from Windows memory.**

**Evidence Collected**

- Windows MiniDump.
- UTF-16LE strings.
- Candidate password fragments.
- KeePass runtime metadata.

---

## Phase 3 — Vault Authentication

<img src="assets/03_keepass_vault_recovery.png" width="100%">

**Figure 03 — KeePassXC CLI validates successful authentication into the recovered password vault.**

Sensitive credentials remain intentionally **redacted** in this public portfolio edition.

---

# 🎯 Why This Investigation Matters

Unlike exploitation-based CTFs, **Extracted** demonstrates the workflow used by **Digital Forensics & Incident Response analysts** investigating credential theft.

<table>
<tr>
<td width="50%">

### Offensive Perspective

- PowerShell execution.
- Process dumping.
- Vault theft.
- Obfuscation.
- Credential collection.

</td>

<td width="50%">

### Defensive Perspective

- Packet capture analysis.
- Malware reverse engineering.
- Memory forensics.
- IOC extraction.
- Evidence correlation.

</td>
</tr>
</table>

---

# 🛠️ Technologies Used

<table>
<tr>
<td width="33%" align="center">

### Network Analysis

Wireshark

TShark

TCP Streams

</td>

<td width="33%" align="center">

### Malware Analysis

PowerShell

Python 3

Base64

XOR

</td>

<td width="33%" align="center">

### Memory Analysis

KeePassXC CLI

John the Ripper

Windows MiniDump

</td>
</tr>
</table>

---

---

# 🌐 Phase 1 — Network Forensics Investigation

> *Every investigation begins with evidence. In this room, the first evidence source is a captured network packet trace.*

---

## 📡 Initial Evidence Acquisition

The investigation starts with a **PCAPNG** file captured from a Windows workstation communicating with an attacker-controlled host.

Rather than attacking a live machine, the analyst must reconstruct the attack **entirely from network traffic**.

<div align="center">

| Evidence Source | Investigation Goal |
|:--:|:--:|
| 🌐 HTTP Traffic | Identify malicious payload delivery |
| 📦 TCP Streams | Recover encoded artifacts |
| 💻 Host Metadata | Correlate PowerShell execution |

</div>

---

## 🔎 Investigation Strategy

<table>
<tr>
<td width="50%">

### Questions Asked

- What protocols exist?
- Is PowerShell visible?
- Were files downloaded?
- Which TCP ports are suspicious?
- Which streams contain binary payloads?

</td>

<td width="50%">

### Evidence Outcome

- HTTP PowerShell delivery discovered.
- TCP/1337 identified.
- TCP/1338 identified.
- Encoded payloads recovered.
- Exfiltration workflow reconstructed.

</td>
</tr>
</table>

---

## 📸 Evidence — HTTP PowerShell Delivery

<img src="assets/01_wireshark_powershell_delivery.png" width="100%">

<div align="center">

**Figure 01 — Wireshark identifies the malicious PowerShell payload delivered through HTTP.**

</div>

---

## 🧩 Wireshark Evidence Breakdown

| Packet Observation | Investigation Finding |
|--------------------|----------------------|
| HTTP GET Request | Downloads PowerShell script |
| `.ps1` Filename | Suspicious executable payload |
| Windows PowerShell User-Agent | Script executed from PowerShell |
| HTTP Response Body | Contains malicious PowerShell code |
| Internal Lab Host | Payload source identified |

---

## 🛰️ Network Traffic Analysis Workflow

```text
PCAPNG Evidence
      │
      ▼
HTTP Traffic Filter
      │
      ▼
PowerShell Request
      │
      ▼
Inspect Response Body
      │
      ▼
Identify Payload Behaviour
      │
      ▼
Pivot Into TCP Streams
```

---

## 🎯 Network Forensics Findings

<table>
<tr>
<th width="35%">Finding</th>
<th>Security Significance</th>
</tr>

<tr>
<td>PowerShell downloaded over HTTP</td>
<td>Initial execution vector identified.</td>
</tr>

<tr>
<td>Separate outbound TCP channels</td>
<td>Artifact exfiltration detected.</td>
</tr>

<tr>
<td>Encoded ASCII payloads</td>
<td>Binary data transported through Base64.</td>
</tr>

<tr>
<td>Uncommon TCP ports</td>
<td>Potential attacker-controlled listeners.</td>
</tr>

</table>

---

# ⚡ Phase 2 — PowerShell Malware Reverse Engineering

> *Static malware analysis reveals attacker intent before any memory investigation begins.*

---

## 🧠 Why Reverse Engineer the Payload?

The PowerShell payload answers questions that packet analysis alone cannot:

- Which files were stolen?
- Which process was targeted?
- How were artifacts encoded?
- Which network ports contain useful evidence?

---

## ⚙️ Malware Behaviour Overview

<div align="center">

```text
PowerShell Execution
        │
        ▼
Locate KeePass Process
        │
        ├────────────► Dump Process Memory
        │
        └────────────► Read KeePass Database
                        │
                        ▼
              XOR + Base64 Encoding
                        │
                        ▼
         TCP Port 1337 / TCP Port 1338
```

</div>

---

## 🛠️ Malware Capability Matrix

<table>
<tr>
<th>PowerShell Capability</th>
<th>Investigation Value</th>
</tr>

<tr>
<td>Process Enumeration</td>
<td>Identifies KeePass process.</td>
</tr>

<tr>
<td>Memory Dump Creation</td>
<td>Captures runtime credentials.</td>
</tr>

<tr>
<td>File Access</td>
<td>Reads encrypted KeePass vault.</td>
</tr>

<tr>
<td>XOR Encoding</td>
<td>Lightweight payload obfuscation.</td>
</tr>

<tr>
<td>Base64 Encoding</td>
<td>ASCII-safe transmission.</td>
</tr>

<tr>
<td>TCP Communication</td>
<td>Artifact exfiltration.</td>
</tr>

</table>

---

## 🔒 Attacker Obfuscation Workflow

<table>
<tr>
<td width="50%">

### KeePass Memory Dump

```text
MiniDump
   │
   ▼
XOR (0x41)
   │
   ▼
Base64
   │
   ▼
TCP/1337
```

</td>

<td width="50%">

### KeePass Database

```text
Database1337.kdbx
       │
       ▼
 XOR (0x42)
       │
       ▼
 Base64
       │
       ▼
 TCP/1338
```

</td>
</tr>
</table>

---

## 🛡️ Malware Analysis Findings

| Finding | DFIR Importance |
|----------|----------------|
| KeePass targeted specifically. | Credential theft objective. |
| XOR keys embedded in script. | Artifact recovery possible. |
| TCP destination ports exposed. | Stream reconstruction simplified. |
| Base64 used instead of encryption. | Payload reversible. |

---

# 🌊 Phase 3 — TCP Stream Reconstruction

> *TCP stream reconstruction converts fragmented network traffic into recoverable forensic evidence.*

---

## 📦 Why Follow TCP Stream?

Wireshark reconstructs complete conversations by:

- Ordering TCP packets.
- Removing retransmissions.
- Preserving binary payload order.
- Exporting clean raw evidence.

---

## 🔄 TCP Stream Recovery Workflow

```text
Captured Packets
      │
      ▼
Filter TCP Port
      │
      ▼
Follow TCP Stream
      │
      ▼
Export RAW Payload
      │
      ▼
Recovered Artifact Stream
```

---

## 📊 Investigation Streams

<table>
<tr>
<th>TCP Port</th>
<th>Recovered Artifact</th>
</tr>

<tr>
<td><code>1337</code></td>
<td>KeePass Process Dump</td>
</tr>

<tr>
<td><code>1338</code></td>
<td>KeePass Database</td>
</tr>

<tr>
<td><code>1339</code></td>
<td>PowerShell Delivery</td>
</tr>

</table>

---

## 🧰 Wireshark Recovery Procedure

<div align="center">

| Step | Investigator Action |
|:---:|---------------------|
| **1** | Locate packet using TCP filter. |
| **2** | Right-click packet. |
| **3** | Follow → TCP Stream. |
| **4** | Change output format to **Raw**. |
| **5** | Save payload to disk. |

</div>

---

## 💻 CLI Recovery with TShark

### Extract TCP Payload

```bash
tshark -r traffic.pcapng \
-Y "tcp.port == 1338" \
-T fields \
-e tcp.payload | tr -d '\n'
```

### Convert Hexadecimal Stream

```bash
xxd -r -p combined_hex_1338.txt > stream_1338.raw
```

Repeat for TCP port **1337**.

---

## 📁 Artifact Recovery Summary

<table>
<tr>
<th>Recovered Stream</th>
<th>Purpose</th>
</tr>

<tr>
<td><code>stream_1337.raw</code></td>
<td>Encoded Windows process dump.</td>
</tr>

<tr>
<td><code>stream_1338.raw</code></td>
<td>Encoded KeePass database.</td>
</tr>

</table>

---

## 🚨 Common DFIR Pitfalls

<table>
<tr>
<th>Mistake</th>
<th>Correct Investigation Practice</th>
</tr>

<tr>
<td>Export packets individually.</td>
<td>Export the reconstructed TCP stream.</td>
</tr>

<tr>
<td>Decode packet payloads manually.</td>
<td>Use Follow TCP Stream → Raw.</td>
</tr>

<tr>
<td>Ignore retransmissions.</td>
<td>Allow Wireshark to reconstruct conversations.</td>
</tr>

</table>

---

# 🧬 Phase 4 — Artifact Recovery Pipeline

> *Reverse attacker encoding to recover original forensic evidence.*

---

## 🔄 Reconstruction Pipeline

<div align="center">

```text
TCP Stream
    │
    ▼
ASCII Base64 Payload
    │
    ▼
Base64 Decode
    │
    ▼
Reverse XOR
    │
    ▼
Recovered Binary Artifact
```

</div>

---

## 🔐 Why XOR Was Used

The attacker used **single-byte XOR** rather than encryption.

### Investigation Perspective

| XOR Benefit for Attacker | Investigation Response |
|---------------------------|------------------------|
| Lightweight obfuscation. | Reverse using same key. |
| Avoid plaintext signatures. | Recover bytes after Base64 decoding. |
| Minimal implementation effort. | Easy forensic reconstruction. |

---

## ⚙️ Recovery Workflow

<table>
<tr>
<td width="50%">

### Process Dump

```text
stream_1337.raw
      │
      ▼
Base64 Decode
      │
      ▼
XOR 0x41
      │
      ▼
keepprocess.dmp
```

</td>

<td width="50%">

### KeePass Database

```text
stream_1338.raw
      │
      ▼
Base64 Decode
      │
      ▼
XOR 0x42
      │
      ▼
Database1337.kdbx
```

</td>
</tr>
</table>

---

## 🐍 Recovery Script (Concept)

```python
decoded = base64.b64decode(stream)

recovered = bytes(byte ^ xor_key for byte in decoded)
```

A single helper script reconstructs both artifacts using different XOR keys.

---

## 📦 Artifact Validation

Immediately validate recovered binaries before continuing.

<table>
<tr>
<th>Validation Tool</th>
<th>Purpose</th>
</tr>

<tr>
<td><code>file</code></td>
<td>Identify recovered artifact type.</td>
</tr>

<tr>
<td><code>strings</code></td>
<td>Inspect embedded strings.</td>
</tr>

<tr>
<td><code>strings -el</code></td>
<td>Extract UTF-16LE strings from Windows memory.</td>
</tr>

</table>

---

## 📁 Recovered Investigation Artifacts

| Artifact | Investigation Role |
|----------|-------------------|
| `Database1337.kdbx` | Encrypted credential vault recovered from network traffic. |
| `keepprocess.dmp` | Windows process memory recovered from attacker exfiltration. |

---

# 💾 KeePass Attack Architecture

> *Understanding KeePass explains why attackers steal both disk and memory artifacts.*

---

## 🧩 Credential Theft Architecture

<div align="center">

```text
             User Unlocks KeePass
                      │
                      ▼
            Master Password Processed
                      │
         ┌────────────┴────────────┐
         ▼                         ▼
Encrypted Database           Runtime Memory
(.kdbx on Disk)            (KeePass Process RAM)
         │                         │
         ▼                         ▼
Encrypted Secrets          Temporary Plaintext Material
         │                         │
         └────────────┬────────────┘
                      ▼
              Attacker Collects Both
```

</div>

---

## 🔒 KeePass Security Model

<table>
<tr>
<th>Component</th>
<th>Purpose</th>
</tr>

<tr>
<td>KDBX Database</td>
<td>Encrypted credential storage.</td>
</tr>

<tr>
<td>Master Password</td>
<td>Primary authentication secret.</td>
</tr>

<tr>
<td>KDF (Argon2/AES)</td>
<td>Derives encryption key.</td>
</tr>

<tr>
<td>Composite Key</td>
<td>Optional multi-factor vault protection.</td>
</tr>

<tr>
<td>Process Memory</td>
<td>Runtime credential material.</td>
</tr>

</table>

---

## 🎯 Why Memory Changes Everything

<table>
<tr>
<td width="50%">

### Database Alone

- Strong encryption.
- Offline cracking required.
- Computationally expensive.

</td>

<td width="50%">

### Database + Memory Dump

- Runtime password material.
- Candidate key recovery.
- Direct vault validation possible.

</td>
</tr>
</table>

---

## 📸 Memory Analysis Evidence

<img src="assets/02_keepass_dump_extraction.png" width="100%">

<div align="center">

**Figure 02 — KeePass process memory analyzed to recover candidate credential material.**

</div>

---

## 🔍 Investigation Status

<table>
<tr>
<th>Completed Phase</th>
<th>Status</th>
</tr>

<tr>
<td>Network Packet Analysis</td>
<td>✅ Completed</td>
</tr>

<tr>
<td>PowerShell Reverse Engineering</td>
<td>✅ Completed</td>
</tr>

<tr>
<td>TCP Stream Reconstruction</td>
<td>✅ Completed</td>
</tr>

<tr>
<td>Artifact Recovery</td>
<td>✅ Completed</td>
</tr>

<tr>
<td>KeePass Memory Investigation</td>
<td>✅ Evidence Recovered</td>
</tr>

</table>

---

---

# 🧠 Phase 5 — Windows Memory Forensics

> *The investigation transitions from network evidence into volatile memory analysis to recover credential material from a running KeePass process.*

---

<div align="center">

## 💾 MEMORY FORENSICS DASHBOARD

<table>
<tr>
<td align="center" width="25%">

### 🧩 Memory Type

Windows MiniDump

</td>

<td align="center" width="25%">

### 🎯 Target Process

KeePass.exe

</td>

<td align="center" width="25%">

### 🔍 Encoding

UTF-16LE Strings

</td>

<td align="center" width="25%">

### ✅ Objective

Recover Runtime Secrets

</td>
</tr>
</table>

</div>

---

## Why Analyze Memory Instead of the Database?

The encrypted KeePass database alone is designed to resist offline attacks.

The attacker therefore targets **volatile memory**, where sensitive information temporarily exists while the vault is unlocked.

<table>
<tr>
<td width="50%">

### 🔐 Encrypted Database

- AES encrypted.
- Stored on disk.
- Requires master password.
- Strong KDF protection.

</td>

<td width="50%">

### 💾 Runtime Memory

- Processed master password.
- Temporary decrypted strings.
- Session secrets.
- Encryption key material.

</td>
</tr>
</table>

---

## KeePass Memory Architecture

```text
              User Unlocks KeePass
                       │
                       ▼
            Master Password Processed
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
   Encrypted Database          Runtime Process Memory
      Database1337.kdbx          KeePass.exe Memory
         │                           │
         ▼                           ▼
Encrypted Vault Data        Temporary Secret Material
         │                           │
         └─────────────┬─────────────┘
                       ▼
             Windows MiniDump Created
                       ▼
              Memory Credential Recovery
```

---

## Memory Acquisition Workflow

The recovered PowerShell payload creates a Windows **MiniDump** of the running KeePass process.

### Memory Acquisition Timeline

| Investigation Stage | Evidence Produced |
|---------------------|-------------------|
| KeePass running | Runtime secrets exist |
| PowerShell executed | Process dump generated |
| XOR applied | Obfuscated binary |
| Base64 encoded | Network transport payload |
| TCP Stream recovered | Original dump reconstructed |

---

## Evidence — KeePass Memory Dump

<img src="assets/img/02_keepass_dump_extraction.png" width="100%">

<div align="center">

**Figure 02 — Memory dump inspection identifying candidate credential material from KeePass process memory.**

</div>

---

## Memory Validation Pipeline

<table>
<tr>
<td width="50%">

### Binary Validation

```bash
file keepprocess.dmp
```

Confirms Windows MiniDump format.

</td>

<td width="50%">

### Unicode Extraction

```bash
strings -el keepprocess.dmp
```

Extract UTF-16LE strings used by Windows.

</td>
</tr>
</table>

---

## DFIR Observations

<table>
<tr>
<th>Recovered Evidence</th>
<th>Investigation Value</th>
</tr>

<tr>
<td>KeePass executable references</td>
<td>Confirms correct target process.</td>
</tr>

<tr>
<td>UTF-16LE strings</td>
<td>Candidate password fragments recovered.</td>
</tr>

<tr>
<td>Module metadata</td>
<td>Runtime process validation.</td>
</tr>

<tr>
<td>Heap memory strings</td>
<td>Credential investigation pivot.</td>
</tr>

</table>

---

# 🔐 Phase 6 — Credential Recovery Methodology

> *Recovered memory evidence is validated cryptographically rather than trusted blindly.*

---

## Credential Recovery Workflow

```text
Recovered MiniDump
        │
        ▼
Unicode String Extraction
        │
        ▼
Candidate Password
        │
        ▼
Targeted Candidate Generation
        │
        ▼
John the Ripper Validation
        │
        ▼
Validated KeePass Password
```

---

## Why Candidate Generation?

Memory dumps may contain partially reconstructed strings.

Instead of brute forcing the vault:

- Preserve recovered characters.
- Replace unresolved characters.
- Generate a bounded candidate list.
- Validate cryptographically.

---

## Validation Strategy

<table>
<tr>
<td width="50%">

### Candidate Creation

```text
Recovered Candidate
      │
      ▼
Unknown Character
      │
      ▼
Generate Candidates
```

</td>

<td width="50%">

### Cryptographic Validation

```text
Candidates
     │
     ▼
keepass2john
     │
     ▼
John the Ripper
```

</td>
</tr>
</table>

---

## Password Validation Pipeline

<table>
<tr>
<th>Tool</th>
<th>Purpose</th>
</tr>

<tr>
<td><code>keepass2john</code></td>
<td>Convert KDBX database into John hash format.</td>
</tr>

<tr>
<td><code>john</code></td>
<td>Validate candidate password cryptographically.</td>
</tr>

<tr>
<td><code>keepassxc-cli</code></td>
<td>Authenticate into recovered vault.</td>
</tr>

</table>

---

## Portfolio Safe Validation

<table>
<tr>
<th>Sensitive Item</th>
<th>Public Repository</th>
</tr>

<tr>
<td>Master Password</td>
<td><code>[REDACTED]</code></td>
</tr>

<tr>
<td>Recovered Flag</td>
<td><code>THM&#123;FLAG_REDACTED&#125;</code></td>
</tr>

<tr>
<td>Vault UUID</td>
<td><code>[REDACTED]</code></td>
</tr>

</table>

---

# 🗃️ Phase 7 — KeePass Vault Authentication

> *Recovered credentials successfully authenticate into the encrypted password vault.*

---

<div align="center">

## 🔑 VAULT AUTHENTICATION STATUS

<table>
<tr>
<td align="center" width="25%">

### Vault

Recovered

</td>

<td align="center" width="25%">

### Password

Validated

</td>

<td align="center" width="25%">

### Authentication

Successful

</td>

<td align="center" width="25%">

### Secrets

Redacted

</td>
</tr>
</table>

</div>

---

## Evidence — KeePass Vault Recovery

<img src="assets/03_keepass_vault_recovery.png" width="100%">

<div align="center">

**Figure 03 — KeePassXC CLI successfully authenticates into the recovered vault after credential validation.**

</div>

---

## Vault Investigation Summary

<table>
<tr>
<th>Vault Evidence</th>
<th>Status</th>
</tr>

<tr>
<td>Encrypted Database Recovered</td>
<td>✅</td>
</tr>

<tr>
<td>Password Validated</td>
<td>✅</td>
</tr>

<tr>
<td>Vault Authenticated</td>
<td>✅</td>
</tr>

<tr>
<td>Challenge Entry Located</td>
<td>✅</td>
</tr>

<tr>
<td>Challenge Flag Published</td>
<td>❌ Redacted</td>
</tr>

</table>

---

## Credential Recovery Architecture

```text
KeePass Database
      │
      ▼
KDBX Validation
      │
      ▼
Master Password Candidate
      │
      ▼
Cryptographic Verification
      │
      ▼
Vault Authentication
      │
      ▼
Protected Entry Access
```

---

# 📊 Investigation Evidence Dashboard

---

<div align="center">

## 📦 RECOVERED FORENSIC ARTIFACTS

</div>

<table>
<tr>
<th width="35%">Recovered Artifact</th>
<th>Description</th>
</tr>

<tr>
<td><code>traffic.pcapng</code></td>
<td>Primary forensic evidence used throughout the investigation.</td>
</tr>

<tr>
<td><code>xxxmmdcclxxxiv.ps1</code></td>
<td>Recovered PowerShell malware payload.</td>
</tr>

<tr>
<td><code>stream_1337.raw</code></td>
<td>Encoded TCP payload containing Windows memory dump.</td>
</tr>

<tr>
<td><code>stream_1338.raw</code></td>
<td>Encoded TCP payload containing KeePass vault.</td>
</tr>

<tr>
<td><code>keepprocess.dmp</code></td>
<td>Recovered Windows MiniDump.</td>
</tr>

<tr>
<td><code>Database1337.kdbx</code></td>
<td>Recovered KeePass credential vault.</td>
</tr>

</table>

---

## Investigation Success Matrix

<table>
<tr>
<th>Investigation Objective</th>
<th>Status</th>
</tr>

<tr>
<td>HTTP Payload Identified</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>PowerShell Payload Reversed</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>TCP Streams Reconstructed</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>Artifacts Decoded</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>Windows Memory Dump Validated</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>Credential Candidate Recovered</td>
<td>🟢 Complete</td>
</tr>

<tr>
<td>KeePass Vault Authenticated</td>
<td>🟢 Complete</td>
</tr>

</table>

---

# 🎯 Indicators of Compromise (IOC Dashboard)

<div align="center">

## IOC SUMMARY

<table>
<tr>
<td align="center" width="20%">

### HTTP

Payload Delivery

</td>

<td align="center" width="20%">

### TCP

1337 / 1338 / 1339

</td>

<td align="center" width="20%">

### Malware

PowerShell

</td>

<td align="center" width="20%">

### Memory

MiniDump

</td>

<td align="center" width="20%">

### Vault

KeePass KDBX

</td>
</tr>
</table>

</div>

---

## Network Indicators

<table>
<tr>
<th>Indicator</th>
<th>Evidence</th>
</tr>

<tr>
<td>HTTP PowerShell Download</td>
<td>Initial malware delivery.</td>
</tr>

<tr>
<td>PowerShell User-Agent</td>
<td>Windows PowerShell execution.</td>
</tr>

<tr>
<td>TCP Port 1337</td>
<td>Process dump exfiltration.</td>
</tr>

<tr>
<td>TCP Port 1338</td>
<td>KeePass database exfiltration.</td>
</tr>

<tr>
<td>TCP Port 1339</td>
<td>Payload delivery infrastructure.</td>
</tr>

</table>

---

## Host Indicators

<table>
<tr>
<th>Artifact</th>
<th>Security Significance</th>
</tr>

<tr>
<td><code>Database1337.kdbx</code></td>
<td>Encrypted password vault targeted.</td>
</tr>

<tr>
<td><code>keepprocess.dmp</code></td>
<td>Volatile memory acquisition.</td>
</tr>

<tr>
<td>PowerShell Execution</td>
<td>Malware execution stage.</td>
</tr>

</table>

---

# 🛡️ MITRE ATT&CK Coverage

<div align="center">

## ATT&CK COVERAGE MATRIX

</div>

<table>
<tr>
<th>Tactic</th>
<th>Technique</th>
<th>ID</th>
</tr>

<tr>
<td>Execution</td>
<td>PowerShell</td>
<td><strong>T1059.001</strong></td>
</tr>

<tr>
<td>Execution</td>
<td>Command & Scripting Interpreter</td>
<td><strong>T1059</strong></td>
</tr>

<tr>
<td>Credential Access</td>
<td>OS Credential Dumping</td>
<td><strong>T1003</strong></td>
</tr>

<tr>
<td>Credential Access</td>
<td>Credentials from Password Stores</td>
<td><strong>T1555</strong></td>
</tr>

<tr>
<td>Defense Evasion</td>
<td>Obfuscated Files or Information</td>
<td><strong>T1027</strong></td>
</tr>

<tr>
<td>Collection</td>
<td>Data from Local System</td>
<td><strong>T1005</strong></td>
</tr>

<tr>
<td>Exfiltration</td>
<td>Exfiltration Over C2 Channel</td>
<td><strong>T1041</strong></td>
</tr>

</table>

---

## ATT&CK Attack Lifecycle

```text
Execution
    │
    ▼
PowerShell Payload
    │
    ▼
Credential Collection
    │
    ▼
KeePass Memory Dump
    │
    ▼
Obfuscation (Base64 + XOR)
    │
    ▼
Network Exfiltration
    │
    ▼
Credential Validation
```

---

# 🧪 DFIR Findings

<table>
<tr>
<th width="40%">Evidence Source</th>
<th>Investigation Result</th>
</tr>

<tr>
<td>Packet Capture</td>
<td>PowerShell delivery identified.</td>
</tr>

<tr>
<td>PowerShell Script</td>
<td>Credential theft methodology discovered.</td>
</tr>

<tr>
<td>TCP Streams</td>
<td>Encoded artifacts reconstructed.</td>
</tr>

<tr>
<td>Windows MiniDump</td>
<td>Candidate password recovered.</td>
</tr>

<tr>
<td>KeePass Database</td>
<td>Vault successfully authenticated.</td>
</tr>

</table>

---

---

# 🛡️ Phase 8 — Detection Engineering & Blue Team Analysis

> *This investigation is not only about recovering attacker artifacts — it also demonstrates how defenders can detect similar activity inside enterprise environments.*

---

<div align="center">

# 🎯 BLUE TEAM DEFENSE DASHBOARD

<table>
<tr>
<td align="center" width="25%">

### 🚨 Execution

PowerShell Detection

</td>

<td align="center" width="25%">

### 💾 Credential Access

KeePass Memory Protection

</td>

<td align="center" width="25%">

### 🌐 Network Detection

Base64 Exfiltration

</td>

<td align="center" width="25%">

### 📊 SOC Hunting

EDR + SIEM Queries

</td>
</tr>
</table>

</div>

---

## Detection Coverage Matrix

| Attack Stage | Detection Opportunity | Security Visibility |
|---------------|-----------------------|---------------------|
| PowerShell Download | Script Block Logging | 🟢 High |
| PowerShell Execution | Sysmon Process Creation | 🟢 High |
| KeePass Process Access | EDR Memory Monitoring | 🟢 High |
| Process Dump Creation | Sysmon Event ID 10 | 🟢 High |
| Base64 Payload Transmission | Network IDS / Proxy Logs | 🟡 Medium |
| TCP Port 1337 / 1338 | Firewall / NetFlow Monitoring | 🟢 High |
| KeePass Vault Access | Endpoint Telemetry | 🟡 Medium |

---

# 🚨 Sigma Detection Opportunities

Professional SOC teams can detect multiple behaviors observed in this investigation.

---

## Sigma Rule — Suspicious PowerShell Download

```yaml
title: Suspicious PowerShell HTTP Download

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    Image|endswith:
      - powershell.exe
    CommandLine|contains:
      - http
      - .ps1

condition: selection

level: high
```

**Purpose**

Detect PowerShell downloading remote scripts over HTTP.

---

## Sigma Rule — KeePass Memory Dump

```yaml
title: KeePass Process Memory Dump

logsource:
  product: windows
  category: process_access

detection:
  selection:
    TargetImage|contains:
      - KeePass.exe

condition: selection

level: high
```

**Purpose**

Detect unauthorized access to KeePass process memory.

---

## Sigma Rule — Encoded PowerShell Execution

```yaml
title: Encoded PowerShell Command

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    CommandLine|contains:
      - -EncodedCommand
      - FromBase64String

condition: selection

level: medium
```

**Purpose**

Detect Base64 encoded PowerShell execution attempts.

---

# 🛰️ Threat Hunting Opportunities

> Hunting combines endpoint telemetry with network evidence collected during the investigation.

---

## Microsoft Defender XDR (KQL)

### Hunt PowerShell Script Downloads

```kusto
DeviceProcessEvents
| where ProcessCommandLine contains ".ps1"
| where ProcessCommandLine contains "http"
```

---

### Hunt KeePass Process Access

```kusto
DeviceEvents
| where AdditionalFields contains "KeePass"
```

---

### Hunt Suspicious PowerShell

```kusto
DeviceProcessEvents
| where FileName =~ "powershell.exe"
| where ProcessCommandLine contains "Base64"
```

---

## Splunk Hunting Example

```spl
index=windows EventCode=4688
Image="*powershell.exe"
CommandLine="*.ps1*"
```

---

## Elastic / Sysmon Hunting

```kql
process.name : "powershell.exe" and
process.command_line : "*.ps1*"
```

---

# 📡 Network Detection Opportunities

<table>
<tr>
<th>Indicator</th>
<th>Defensive Recommendation</th>
</tr>

<tr>
<td>HTTP-delivered PowerShell payload</td>
<td>Inspect downloaded PowerShell scripts.</td>
</tr>

<tr>
<td>Large Base64 TCP payloads</td>
<td>Alert on unusually large outbound Base64 traffic.</td>
</tr>

<tr>
<td>Ports 1337 / 1338</td>
<td>Investigate uncommon outbound TCP destinations.</td>
</tr>

<tr>
<td>PowerShell User-Agent</td>
<td>Baseline legitimate PowerShell network activity.</td>
</tr>

</table>

---

# 🔐 Credential Protection Recommendations

<table>
<tr>
<th>Recommendation</th>
<th>Security Benefit</th>
</tr>

<tr>
<td>Use a strong KeePass master password.</td>
<td>Improves resistance against offline attacks.</td>
</tr>

<tr>
<td>Use Argon2 KDF.</td>
<td>Increases password derivation cost.</td>
</tr>

<tr>
<td>Enable MFA where possible.</td>
<td>Additional authentication layer.</td>
</tr>

<tr>
<td>Lock KeePass when idle.</td>
<td>Reduces runtime memory exposure.</td>
</tr>

<tr>
<td>Disable clipboard history.</td>
<td>Limits plaintext credential persistence.</td>
</tr>

</table>

---

# 🧪 Phase 9 — Incident Response Playbook

> *A structured incident response workflow based on the evidence recovered during this investigation.*

---

<div align="center">

# 🚑 DFIR RESPONSE PLAYBOOK

</div>

## Phase 1 — Detection

<table>
<tr>
<td width="60">

### 1️⃣

</td>

<td>

- Alert received from network telemetry.
- Suspicious PowerShell communication identified.
- Preserve packet capture evidence.

</td>
</tr>
</table>

---

## Phase 2 — Containment

<table>
<tr>
<td width="60">

### 2️⃣

</td>

<td>

- Isolate compromised workstation.
- Preserve volatile memory.
- Stop outbound connections.
- Collect PowerShell logs.

</td>
</tr>
</table>

---

## Phase 3 — Investigation

<table>
<tr>
<td width="60">

### 3️⃣

</td>

<td>

- Recover PowerShell payload.
- Extract TCP streams.
- Decode artifacts.
- Validate memory dump.
- Correlate evidence.

</td>
</tr>
</table>

---

## Phase 4 — Eradication

<table>
<tr>
<td width="60">

### 4️⃣

</td>

<td>

- Remove malicious PowerShell payload.
- Rotate compromised credentials.
- Replace KeePass vault if necessary.
- Review endpoint persistence.

</td>
</tr>
</table>

---

## Phase 5 — Recovery

<table>
<tr>
<td width="60">

### 5️⃣

</td>

<td>

- Restore trusted credential vault.
- Re-enable workstation.
- Continue monitoring endpoint telemetry.

</td>
</tr>
</table>

---

# 📊 Incident Response Timeline

```text
ALERT RECEIVED
      │
      ▼
Packet Capture Preserved
      │
      ▼
PowerShell Payload Investigated
      │
      ▼
TCP Streams Reconstructed
      │
      ▼
Artifacts Recovered
      │
      ▼
Memory Analysis
      │
      ▼
Credential Validation
      │
      ▼
Incident Confirmed
      │
      ▼
Recovery & Hardening
```

---

# 🧾 Evidence Correlation Dashboard

<table>
<tr>
<th>Evidence Source</th>
<th>Investigation Result</th>
</tr>

<tr>
<td>Packet Capture</td>
<td>HTTP PowerShell delivery identified.</td>
</tr>

<tr>
<td>PowerShell Payload</td>
<td>Credential theft workflow reconstructed.</td>
</tr>

<tr>
<td>TCP Streams</td>
<td>Encoded KeePass artifacts recovered.</td>
</tr>

<tr>
<td>Memory Dump</td>
<td>Candidate credential material extracted.</td>
</tr>

<tr>
<td>KeePass Database</td>
<td>Encrypted vault authenticated successfully.</td>
</tr>

<tr>
<td>Credential Validation</td>
<td>Challenge objective completed without exposing secrets.</td>
</tr>

</table>

---

# 📚 Lessons Learned

<div align="center">

## 🧠 DIGITAL FORENSICS TAKEAWAYS

</div>

<table>
<tr>
<td width="50%">

### 🌐 Network Forensics

- PCAP analysis reveals attacker infrastructure.
- TCP streams reconstruct complete evidence.
- HTTP payload inspection identifies malware delivery.
- Network metadata guides the investigation.

</td>

<td width="50%">

### 💾 Memory Forensics

- Runtime memory complements disk artifacts.
- UTF-16LE extraction is critical on Windows.
- Process dumps may expose authentication material.
- Validate memory-derived evidence cryptographically.

</td>
</tr>

<tr>
<td>

### ⚙️ Malware Analysis

- Static analysis reveals attacker methodology.
- XOR is obfuscation, not encryption.
- Base64 is reversible transport encoding.
- Scripts often expose recovery logic.

</td>

<td>

### 🛡️ Incident Response

- Correlate host + network evidence.
- Validate every recovered artifact.
- Preserve volatile memory early.
- Build detections from observed attacker behavior.

</td>
</tr>
</table>

---

# 🎯 Skills Demonstrated

<div align="center">

## CYBERSECURITY SKILL MATRIX

</div>

<table>
<tr>
<th width="35%">Domain</th>
<th>Skills Practiced</th>
</tr>

<tr>
<td>Digital Forensics</td>
<td>Evidence collection, artifact validation, timeline reconstruction.</td>
</tr>

<tr>
<td>Network Security</td>
<td>Wireshark analysis, TCP stream reconstruction, HTTP investigation.</td>
</tr>

<tr>
<td>Malware Analysis</td>
<td>PowerShell reverse engineering, XOR decoding, Base64 reconstruction.</td>
</tr>

<tr>
<td>Memory Forensics</td>
<td>Windows MiniDump analysis, Unicode extraction, credential recovery.</td>
</tr>

<tr>
<td>Blue Team</td>
<td>IOC extraction, Sigma detection logic, threat hunting ideas.</td>
</tr>

<tr>
<td>Incident Response</td>
<td>Evidence correlation, detection opportunities, remediation workflow.</td>
</tr>

</table>

---

# 🧰 Tools Used Throughout the Investigation

<table>
<tr>
<th>Category</th>
<th>Tools</th>
</tr>

<tr>
<td>Network Analysis</td>
<td>Wireshark, TShark</td>
</tr>

<tr>
<td>Malware Analysis</td>
<td>PowerShell, Python 3</td>
</tr>

<tr>
<td>Memory Analysis</td>
<td>Windows MiniDump, strings, UTF-16LE extraction</td>
</tr>

<tr>
<td>Password Analysis</td>
<td>KeePassXC CLI, John the Ripper</td>
</tr>

<tr>
<td>Documentation</td>
<td>Markdown, GitHub Pages, Jekyll Hacker Theme</td>
</tr>

</table>

---

# 📖 References & Learning Resources

This investigation is based on concepts commonly used in Digital Forensics and Incident Response.

### Official Documentation

- Microsoft PowerShell Documentation
- Microsoft Windows MiniDump Documentation
- Wireshark User Guide
- TShark Documentation
- KeePass Documentation
- KeePassXC Documentation
- MITRE ATT&CK Framework

### Educational Platforms

- TryHackMe — Digital Forensics Learning Path
- Windows Incident Response resources
- Network Forensics learning materials

---

# ⚖️ Responsible Disclosure

This repository is published for **educational purposes**.

To preserve the integrity of the TryHackMe room, the following information has been intentionally removed from this public repository.

<table>
<tr>
<th>Protected Information</th>
<th>Status</th>
</tr>

<tr>
<td>TryHackMe Flag</td>
<td>🔒 Redacted</td>
</tr>

<tr>
<td>KeePass Master Password</td>
<td>🔒 Redacted</td>
</tr>

<tr>
<td>Vault UUID</td>
<td>🔒 Redacted</td>
</tr>

<tr>
<td>Recovered Credentials</td>
<td>🔒 Redacted</td>
</tr>

</table>

Example:

```text
THM{FLAG_REDACTED}
Master Password: [REDACTED]
Vault UUID: [REDACTED]
```

---

# 🏆 Final Investigation Summary

<div align="center">

# INCIDENT CLOSED ✅

</div>

<table>
<tr>
<td width="50%">

### 📌 Investigation Completed

- HTTP PowerShell delivery identified.
- Malware behavior reconstructed.
- TCP streams recovered.
- XOR & Base64 reversed.
- KeePass database reconstructed.
- Windows memory analyzed.
- Credentials validated.
- Vault authenticated.

</td>

<td width="50%">

### 🛡️ Defensive Outcomes

- IOC extraction completed.
- MITRE ATT&CK mapping completed.
- Detection opportunities identified.
- Incident response workflow documented.
- Blue Team recommendations provided.

</td>
</tr>
</table>

---

# 💼 About This Portfolio Project

This repository is part of my **Cybersecurity Portfolio**, documenting practical investigations performed inside authorized laboratory environments.

The objective is to demonstrate hands-on skills in:

- Digital Forensics
- Incident Response
- Network Security
- Memory Analysis
- Threat Detection
- Security Documentation

Every walkthrough emphasizes **methodology**, **evidence correlation**, and **defensive learning** rather than publishing challenge secrets.

---

<div align="center">

# 👨‍💻 Anurag

### Cybersecurity • Digital Forensics • SOC • Threat Detection

<img src="https://img.shields.io/badge/GitHub-anurag--rvnkr1-181717?style=for-the-badge&logo=github"/>

<img src="https://img.shields.io/badge/TryHackMe-Portfolio-red?style=for-the-badge&logo=tryhackme"/>

<img src="https://img.shields.io/badge/GitHub%20Pages-Jekyll%20Hacker-22c55e?style=for-the-badge&logo=jekyll"/>

---

### ⭐ Thank you for visiting this Digital Forensics portfolio project.

**Document • Investigate • Detect • Defend**

</div>
