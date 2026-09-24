---
title: "Extracted — TryHackMe Digital Forensics Walkthrough"
author: "Anurag"
description: "Professional DFIR walkthrough documenting PCAP investigation, PowerShell malware analysis, TCP stream reconstruction, KeePass memory forensics, and credential validation in the Extracted TryHackMe room."
date: 2026-09-24
tags:
  - TryHackMe
  - Digital Forensics
  - DFIR
  - Network Forensics
  - Wireshark
  - PowerShell
  - KeePass
  - Incident Response
---

# 🔍 Extracted — TryHackMe Walkthrough

> **Digital Forensics • Network Traffic Analysis • Memory Forensics • Credential Recovery**

---

<p align="center">
<img src="../docs/assets/01_wireshark_powershell_delivery.png" width="100%">
</p>

<p align="center">
<b>Figure 01 — Initial evidence discovered during packet capture analysis.</b>
</p>

---

## Document Classification

| Property | Value |
|----------|-------|
| **Room Name** | Extracted |
| **Platform** | TryHackMe |
| **Category** | Digital Forensics / Network Analysis |
| **Difficulty** | Medium |
| **Report Type** | Incident Investigation Walkthrough |
| **Environment** | Authorized TryHackMe Laboratory |
| **Documentation Version** | Portfolio Edition v1.0 |
| **Author** | Anurag |

---

# Executive Summary

## Overview

**Extracted** is a Digital Forensics challenge on TryHackMe that simulates a real-world credential theft incident where sensitive information is exfiltrated through an obfuscated PowerShell payload.

Unlike traditional Capture-The-Flag rooms that begin with reconnaissance against a remote service, this room starts with **network evidence**. The investigation begins by inspecting a packet capture (`.pcapng`) and progressively reconstructing attacker behavior through network traffic, encoded artifacts, Windows memory analysis, and encrypted credential storage.

This investigation demonstrates how defenders correlate evidence across multiple forensic sources instead of relying on exploitation alone.

The complete workflow includes:

- Network packet inspection.
- HTTP request analysis.
- PowerShell payload reverse engineering.
- TCP stream reconstruction.
- Base64 decoding.
- XOR deobfuscation.
- KeePass process memory analysis.
- Credential validation.
- Secure vault inspection.

The final objective is achieved through **evidence correlation**, not brute force exploitation.

---

## Investigation Summary

<table>
<tr>
<td width="33%" align="center">

### Evidence Source

**PCAP**

HTTP Traffic

TCP Streams

Encoded Payloads

</td>

<td width="33%" align="center">

### Recovered Artifacts

`.kdbx`

`.dmp`

PowerShell Script

Encoded Streams

</td>

<td width="33%" align="center">

### Final Validation

KeePassXC CLI

Recovered Vault

Redacted Flag

Credential Verification

</td>
</tr>
</table>

---

## Investigation Workflow

```text
                 Network Packet Capture
                         │
                         ▼
              HTTP PowerShell Delivery
                         │
                         ▼
          PowerShell Payload Investigation
                         │
      ┌──────────────────┴─────────────────┐
      ▼                                    ▼
 TCP Stream 1337                     TCP Stream 1338
 KeePass Memory Dump                 KeePass Database
      │                                    │
      ▼                                    ▼
 Base64 Decode                       Base64 Decode
      │                                    │
      ▼                                    ▼
 XOR Reversal                        XOR Reversal
      │                                    │
      ▼                                    ▼
 Windows Minidump                    KeePass Database
              └────────────┬─────────────┘
                           ▼
                Memory Credential Analysis
                           ▼
                Candidate Password Recovery
                           ▼
                  KeePass Vault Validation
                           ▼
               Flag Validation (Redacted)
```

---

# Investigation Objectives

The investigation focuses on reconstructing attacker activity from captured evidence.

### Primary Objectives

- Identify suspicious network activity.
- Recover the malicious PowerShell payload.
- Understand attacker obfuscation techniques.
- Extract exfiltrated artifacts from network traffic.
- Recover encoded Windows memory dump.
- Recover encrypted KeePass vault.
- Identify credential material from memory.
- Authenticate into recovered vault.
- Validate recovered challenge artifact.

---

## Skills Demonstrated

### Network Forensics

- Packet capture analysis.
- HTTP inspection.
- TCP stream reconstruction.
- Payload extraction.
- Traffic filtering.
- Raw binary recovery.

### Malware Analysis

- PowerShell behavioral analysis.
- Script deobfuscation.
- File exfiltration analysis.
- Data transformation analysis.

### Memory Forensics

- Windows Minidump analysis.
- UTF-16LE string extraction.
- Candidate password recovery.
- Credential validation methodology.

### Defensive Security

- IOC identification.
- Evidence correlation.
- Detection engineering.
- Incident response documentation.

---

# Threat Scenario

## Simulated Incident Narrative

A Windows workstation has communicated with an attacker-controlled server. During this communication:

1. A PowerShell script is downloaded.
2. KeePass process memory is dumped.
3. KeePass database is read from disk.
4. Both artifacts are obfuscated.
5. Artifacts are transmitted across multiple TCP ports.
6. The attacker attempts credential theft.

As an incident responder, the objective is to reconstruct exactly what happened.

---

## Threat Model

<table>
<tr>
<th width="35%">Attacker Activity</th>
<th>Defensive Investigation</th>
</tr>

<tr>
<td>Deliver PowerShell payload.</td>
<td>Identify HTTP PowerShell request in PCAP.</td>
</tr>

<tr>
<td>Create KeePass memory dump.</td>
<td>Recover dumped process memory.</td>
</tr>

<tr>
<td>Read encrypted KeePass database.</td>
<td>Recover database from TCP stream.</td>
</tr>

<tr>
<td>Encode artifacts using Base64.</td>
<td>Decode captured payload.</td>
</tr>

<tr>
<td>Obfuscate bytes using XOR.</td>
<td>Reverse XOR transformation.</td>
</tr>

<tr>
<td>Steal credentials.</td>
<td>Recover candidate password and validate vault.</td>
</tr>

</table>

---

# Lab Environment

## Investigation Environment

| Component | Description |
|-----------|-------------|
| Operating System | Windows Workstation (Victim) |
| Investigation Platform | Kali Linux |
| Network Analyzer | Wireshark |
| CLI Analyzer | TShark |
| Scripting Language | Python 3 |
| Password Manager | KeePass |
| Memory Artifact | Windows Minidump |

---

## Evidence Collected

<table>
<tr>
<th>Artifact</th>
<th>Description</th>
</tr>

<tr>
<td><code>traffic.pcapng</code></td>
<td>Primary forensic evidence.</td>
</tr>

<tr>
<td><code>xxxmmdcclxxxiv.ps1</code></td>
<td>Recovered malicious PowerShell payload.</td>
</tr>

<tr>
<td><code>Database1337.kdbx</code></td>
<td>Recovered encrypted KeePass vault.</td>
</tr>

<tr>
<td><code>keepprocess.dmp</code></td>
<td>Recovered Windows process memory dump.</td>
</tr>

<tr>
<td><code>stream_1337.raw</code></td>
<td>Raw TCP stream containing encoded dump.</td>
</tr>

<tr>
<td><code>stream_1338.raw</code></td>
<td>Raw TCP stream containing encoded vault.</td>
</tr>

</table>

---

# Tools Used During Investigation

<table>
<tr>
<th width="28%">Tool</th>
<th>Purpose</th>
</tr>

<tr>
<td><b>Wireshark</b></td>
<td>Packet capture inspection and protocol analysis.</td>
</tr>

<tr>
<td><b>TShark</b></td>
<td>Command-line extraction of TCP payloads.</td>
</tr>

<tr>
<td><b>Python 3</b></td>
<td>Artifact decoding and XOR recovery.</td>
</tr>

<tr>
<td><b>xxd</b></td>
<td>Hexadecimal reconstruction into binary files.</td>
</tr>

<tr>
<td><b>strings</b></td>
<td>Inspect Windows memory dump strings.</td>
</tr>

<tr>
<td><b>KeePassXC CLI</b></td>
<td>Vault inspection and credential validation.</td>
</tr>

<tr>
<td><b>John the Ripper</b></td>
<td>Password candidate validation.</td>
</tr>

</table>

---

# Investigation Timeline

<table>
<tr>
<th width="18%">Phase</th>
<th width="32%">Evidence</th>
<th>Investigator Action</th>
</tr>

<tr>
<td>01</td>
<td>Packet Capture</td>
<td>Inspect HTTP traffic for suspicious requests.</td>
</tr>

<tr>
<td>02</td>
<td>PowerShell Payload</td>
<td>Reverse engineer script behavior.</td>
</tr>

<tr>
<td>03</td>
<td>TCP Streams</td>
<td>Extract encoded artifacts.</td>
</tr>

<tr>
<td>04</td>
<td>Encoded Payload</td>
<td>Reverse Base64 and XOR.</td>
</tr>

<tr>
<td>05</td>
<td>KeePass Artifacts</td>
<td>Recover `.kdbx` and `.dmp`.</td>
</tr>

<tr>
<td>06</td>
<td>Memory Dump</td>
<td>Recover candidate password.</td>
</tr>

<tr>
<td>07</td>
<td>KeePass Vault</td>
<td>Validate recovered credentials.</td>
</tr>

<tr>
<td>08</td>
<td>Challenge Artifact</td>
<td>Verify successful vault access.</td>
</tr>

</table>

---

# Phase 1 — Initial Evidence Collection

## Opening the Packet Capture

The investigation begins with a provided **PCAPNG** file representing captured network traffic from the compromised workstation.

Rather than immediately searching for credentials, the first task is understanding **communication patterns** inside the capture.

### Initial Questions

- Which protocols are present?
- Is HTTP traffic visible?
- Are executable files transferred?
- Which hosts communicate?
- Which TCP ports contain payloads?

These questions guide the investigation before examining payload contents.

---

## Packet Inspection Strategy

The first inspection pass uses protocol filtering.

### Wireshark Filters

```wireshark
http
```

This isolates HTTP requests and responses.

Additional filters used later include:

```wireshark
tcp.port == 1337
```

```wireshark
tcp.port == 1338
```

```wireshark
tcp.port == 1339
```

Each filter corresponds to a separate stage of attacker activity.

---

## HTTP Request Analysis

During HTTP inspection, an outbound request immediately stands out.

Characteristics include:

- Windows PowerShell User-Agent.
- Requesting a `.ps1` file.
- Internal lab host delivering script content.
- Suspicious filename.

This request becomes the primary pivot for the investigation.

<p align="center">
<img src="../docs/assets/01_wireshark_powershell_delivery.png" width="100%">
</p>

**Figure 01 — HTTP GET request delivering the malicious PowerShell payload.**

---

## Initial Observations

### Evidence Summary

| Observation | Significance |
|-------------|-------------|
| `.ps1` downloaded | Indicates PowerShell execution. |
| Windows PowerShell User-Agent | Confirms PowerShell initiated HTTP request. |
| HTTP Response contains script | Payload available for static analysis. |
| Separate TCP ports later observed | Indicates staged exfiltration workflow. |

---

## Why This Matters

PowerShell is frequently used by attackers because it provides:

- Native Windows execution.
- File access.
- Memory interaction.
- Network communication.
- Encoding capabilities.

The presence of PowerShell inside network traffic suggests attacker-controlled automation rather than manual interaction.

---

---

# Phase 2 — PowerShell Payload Reverse Engineering

> **Objective:** Understand exactly what the downloaded PowerShell script does, identify the attacker workflow, locate exfiltration ports, and map every action to the network evidence.

---

## Why Analyze the PowerShell Payload?

Once the HTTP response is identified, the downloaded PowerShell script becomes the **primary source of attacker intent**.

Instead of guessing what was stolen, we inspect the payload to answer four critical DFIR questions:

1. **What files does the malware access?**
2. **How are artifacts modified before exfiltration?**
3. **Where are artifacts transmitted?**
4. **Which network streams contain useful evidence?**

This allows investigators to reconstruct the attack without executing the payload.

---

## Initial Static Analysis

The recovered PowerShell script is inspected as plain text.

### High-Level Behaviour

| Behavior | Purpose |
|----------|---------|
| Locate KeePass process | Identify active password manager session. |
| Dump process memory | Capture runtime credential material. |
| Read KeePass database | Collect encrypted vault from disk. |
| XOR data | Lightweight obfuscation before transmission. |
| Base64 encode | Convert binary to ASCII-safe network payload. |
| HTTP/TCP transmission | Exfiltrate artifacts to attacker listener. |

---

## Payload Behaviour Diagram

```text
Windows Host
     │
     ▼
PowerShell Script Executes
     │
     ├──────────────► Search for KeePass.exe
     │                     │
     │                     ▼
     │             Create Process Dump
     │
     ├──────────────► Read Database1337.kdbx
     │
     ▼
Obfuscate Artifacts
     │
     ├── XOR 0x41 → Process Dump
     └── XOR 0x42 → KeePass Vault
            │
            ▼
       Base64 Encoding
            │
            ▼
 TCP Port 1337 / TCP Port 1338
```

---

## Evidence Correlation

The payload reveals **three independent network channels**.

<table>
<tr>
<th>TCP Port</th>
<th>Observed Purpose</th>
<th>Investigation Priority</th>
</tr>

<tr>
<td><code>1339</code></td>
<td>PowerShell payload delivery.</td>
<td>⭐⭐⭐⭐⭐</td>
</tr>

<tr>
<td><code>1337</code></td>
<td>Encoded KeePass process dump.</td>
<td>⭐⭐⭐⭐⭐</td>
</tr>

<tr>
<td><code>1338</code></td>
<td>Encoded KeePass database.</td>
<td>⭐⭐⭐⭐⭐</td>
</tr>

</table>

This immediately tells investigators **which TCP streams must be reconstructed**.

---

## Indicators Extracted from the Script

### Files Accessed

| File | Purpose |
|------|---------|
| `Database1337.kdbx` | Encrypted KeePass vault. |
| `keepprocess.dmp` | Windows process memory dump. |

### Memory Activity

- Queries running KeePass process.
- Generates process dump.
- Reads binary memory contents.

### Network Activity

- Sends Base64 payloads.
- Uses TCP sockets.
- Splits artifacts across different ports.

---

## Why XOR?

Instead of encrypting data properly, the attacker uses **single-byte XOR**.

### XOR Advantages for Attackers

- Very fast.
- Minimal code.
- Avoids plaintext network signatures.
- Easily reversible during recovery.

### XOR Visualization

```text
Original Byte
      │
      ▼
 XOR with 0x41
      │
      ▼
Encoded Byte
```

The defender reverses the same operation.

---

## Why Base64?

Binary files cannot always be transmitted safely inside ASCII-oriented protocols.

Base64 converts arbitrary binary data into printable text.

### Transformation Pipeline

```text
Original Binary
      │
      ▼
Single-byte XOR
      │
      ▼
Base64 Encoding
      │
      ▼
TCP Payload
```

During recovery:

```text
TCP Payload
      │
      ▼
Base64 Decode
      │
      ▼
Reverse XOR
      │
      ▼
Recovered Binary File
```

---

## DFIR Takeaway

Static malware analysis is not only about identifying malicious behavior.

It also provides:

- IOC extraction.
- Artifact locations.
- Network ports.
- Encoding methods.
- Recovery strategy.

The PowerShell script effectively becomes the blueprint for reconstructing attacker activity.

---

# Phase 3 — Network Stream Reconstruction

> **Objective:** Recover attacker-transmitted artifacts directly from packet capture data.

---

## Understanding TCP Streams

The attacker transmits artifacts over raw TCP connections rather than embedding them into HTTP responses.

Wireshark reconstructs these conversations into continuous byte streams.

### Why Follow TCP Stream?

Advantages include:

- Reassembles fragmented packets.
- Removes retransmission complexity.
- Preserves payload ordering.
- Exports binary safely.

---

## Stream Identification Workflow

```text
PCAP
 │
 ▼
Filter TCP Port
 │
 ▼
Follow TCP Stream
 │
 ▼
Raw Payload Export
 │
 ▼
Recovered Stream File
```

---

## Wireshark Workflow

1. Locate packet using TCP port.
2. Right-click packet.
3. Follow → TCP Stream.
4. Select **Raw**.
5. Save payload.

This produces the cleanest reconstruction.

---

## CLI Workflow with TShark

The same extraction can be automated.

### Extract KeePass Database

```bash
tshark -r traffic.pcapng \
-Y "tcp.port == 1338" \
-T fields \
-e tcp.payload \
| tr -d '\n' > combined_hex_1338.txt
```

### Convert Hex into Binary

```bash
xxd -r -p combined_hex_1338.txt > stream_1338.raw
```

---

### Recover Process Dump Stream

```bash
tshark -r traffic.pcapng \
-Y "tcp.port == 1337" \
-T fields \
-e tcp.payload \
| tr -d '\n' > combined_hex_1337.txt

xxd -r -p combined_hex_1337.txt > stream_1337.raw
```

---

## Why Use Raw Streams?

Packet payloads often contain:

- Segmentation.
- ACK packets.
- Retransmissions.
- Fragmentation.

Raw stream reconstruction eliminates these issues.

---

## Artifact Timeline

<table>
<tr>
<th>Stream</th>
<th>Recovered Output</th>
</tr>

<tr>
<td>TCP Stream — Port 1337</td>
<td><code>stream_1337.raw</code></td>
</tr>

<tr>
<td>TCP Stream — Port 1338</td>
<td><code>stream_1338.raw</code></td>
</tr>

</table>

Both streams still contain encoded content.

---

## Evidence Validation

Before decoding, verify payload size.

```bash
ls -lh stream_1337.raw
ls -lh stream_1338.raw
```

Large payload sizes indicate successful reconstruction.

---

## Common Investigation Mistakes

<table>
<tr>
<th>Mistake</th>
<th>Correct Practice</th>
</tr>

<tr>
<td>Export packet bytes individually.</td>
<td>Use Follow TCP Stream.</td>
</tr>

<tr>
<td>Copy payload manually.</td>
<td>Export Raw stream.</td>
</tr>

<tr>
<td>Decode packets independently.</td>
<td>Reassemble complete TCP conversation.</td>
</tr>

</table>

---

# Phase 4 — Artifact Reconstruction

> **Objective:** Reverse attacker encoding to recover original binary evidence.

---

## Understanding the Encoding Pipeline

The payload performs two reversible transformations.

### Stage 1

Binary artifact.

### Stage 2

XOR obfuscation.

### Stage 3

Base64 encoding.

### Stage 4

Network transmission.

---

## Recovery Pipeline

```text
Network Payload
      │
      ▼
ASCII Base64
      │
      ▼
Base64 Decode
      │
      ▼
XOR Reversal
      │
      ▼
Recovered Artifact
```

---

## Recovery Script

A Python helper automates reconstruction.

```python
import base64

def recover(input_file, output_file, xor_key):
    with open(input_file, "rb") as f:
        raw = f.read()

    text = raw.decode("ascii", errors="ignore").strip()

    decoded = base64.b64decode(text)

    recovered = bytes(b ^ xor_key for b in decoded)

    with open(output_file, "wb") as f:
        f.write(recovered)
```

---

## Recover KeePass Database

```bash
python3 recover.py stream_1338.raw Database1337.kdbx 0x42
```

---

## Recover KeePass Dump

```bash
python3 recover.py stream_1337.raw keepprocess.dmp 0x41
```

---

## Why Decode Before XOR?

Base64 converts binary into text.

Attempting XOR first corrupts the ASCII payload.

Correct order:

1. Base64 decode.
2. Reverse XOR.

---

## Artifact Recovery Summary

<table>
<tr>
<th>Input</th>
<th>Output</th>
</tr>

<tr>
<td><code>stream_1338.raw</code></td>
<td><code>Database1337.kdbx</code></td>
</tr>

<tr>
<td><code>stream_1337.raw</code></td>
<td><code>keepprocess.dmp</code></td>
</tr>

</table>

---

# Phase 5 — Validating Recovered Evidence

> **Objective:** Confirm reconstructed artifacts are genuine before performing forensic analysis.

---

## File Type Validation

Immediately validate recovered binaries.

```bash
file Database1337.kdbx

file keepprocess.dmp
```

Expected outputs identify:

- KeePass database.
- Windows MiniDump.

---

## Binary Inspection

Use strings to identify embedded metadata.

```bash
strings keepprocess.dmp | head
```

Unicode-aware inspection:

```bash
strings -el keepprocess.dmp | head
```

UTF-16LE extraction is particularly useful for Windows memory.

---

## Memory Artifact Indicators

Recovered dump should contain references including:

- MiniDump.
- KeePass executable.
- Module names.
- Unicode strings.
- Runtime buffers.

---

## Why Validate First?

Investigators should never trust decoded artifacts blindly.

Validation ensures:

- Correct XOR key.
- Complete TCP stream.
- Successful Base64 decoding.
- Uncorrupted binary output.

Without validation, later credential recovery may fail for unrelated reasons.

---

## Artifact Evidence

<p align="center">
<img src="../docs/assets/02_keepass_dump_extraction.png" width="100%">
</p>

<p align="center">
<b>Figure 02 — Validation and inspection of the recovered KeePass process dump.</b>
</p>

The recovered process dump is now suitable for memory analysis and credential extraction.

---

---

# Phase 6 — Windows Memory Forensics: KeePass Process Dump Analysis

> **Objective:** Analyze the recovered Windows process memory dump (`keepprocess.dmp`) to identify credential material associated with an active KeePass session.

---

## Introduction to Memory Forensics

Memory forensics is the process of analyzing volatile memory captured from a running operating system or application. Unlike files stored on disk, volatile memory contains **runtime state**, including temporary secrets, decrypted objects, session tokens, and authentication material.

In this investigation, the attacker specifically targeted the **KeePass process memory**, which is significantly more valuable than the encrypted database alone.

This mirrors a common real-world credential theft technique observed during incident response investigations.

---

## Why Target KeePass Memory?

KeePass is designed to protect credentials using a strong encrypted database (`.kdbx`). When the vault is locked, the contents remain encrypted.

However, once a user unlocks the vault:

- Master password is processed in memory.
- Encryption keys are derived.
- Sensitive strings may temporarily exist in RAM.
- Clipboard and UI buffers may contain plaintext values.

Attackers exploit this runtime state by dumping the KeePass process.

### Security Model Comparison

| Data Source | Protection |
|-------------|------------|
| `.kdbx` Database | AES encrypted at rest |
| KeePass Process Memory | Runtime secrets may exist in plaintext or recoverable form |
| Clipboard | Potential temporary plaintext credentials |
| Windows Pagefile | May contain remnants of memory |

---

## Memory Acquisition in the Challenge

The PowerShell payload creates a **Windows MiniDump**.

### MiniDump Characteristics

- Snapshot of process memory.
- Module information.
- Thread state.
- Memory segments.
- Unicode strings.
- Heap allocations.

The recovered artifact is named:

```text
keepprocess.dmp
```

---

## Investigation Workflow

```text
Recovered TCP Stream
        │
        ▼
Reverse Base64
        │
        ▼
Reverse XOR
        │
        ▼
Windows MiniDump
        │
        ▼
Memory Analysis
        │
        ▼
Candidate Password Recovery
```

---

## Initial Artifact Validation

The first investigation step is validating the dump format.

```bash
file keepprocess.dmp
```

Expected identification:

- Microsoft MiniDump
- Windows process dump
- Binary memory artifact

---

## Unicode String Extraction

Windows applications frequently store strings using **UTF-16LE** encoding.

### Standard Strings

```bash
strings keepprocess.dmp
```

### Unicode-Aware Strings

```bash
strings -el keepprocess.dmp
```

Unicode extraction often reveals:

- User interface strings.
- File paths.
- Process names.
- Password fragments.
- KeePass metadata.

---

## Why UTF-16LE Matters

Windows APIs internally use UTF-16LE for many strings.

ASCII extraction alone may miss:

- Password candidates.
- File names.
- Unicode usernames.
- Vault entry labels.

UTF-16LE extraction increases visibility into Windows application memory.

---

## Evidence Collected from Memory

Typical observations include:

| Artifact Type | Investigation Value |
|---------------|--------------------|
| KeePass executable references | Confirms correct process dump |
| DLL module names | Confirms runtime modules |
| Unicode credential fragments | Password investigation |
| Memory allocation structures | Application runtime context |
| Database references | Vault correlation |

---

## Memory Analysis Strategy

Rather than manually inspecting thousands of strings, specialized tooling searches memory structures associated with KeePass.

### Investigation Goals

- Identify candidate master password.
- Locate encryption key material.
- Recover KeePass runtime secrets.
- Validate candidate strings.

---

# KeePass Memory Extraction Methodology

## Specialized Dump Analysis

Purpose-built tools understand KeePass memory layouts and automate extraction.

Example workflow:

```bash
git clone https://github.com/matro7sh/keepass-dump-masterkey.git

cd keepass-dump-masterkey

python3 poc.py ../keepprocess.dmp
```

The parser searches memory for candidate password material.

---

## Evidence Output

<p align="center">
<img src="../docs/assets/img/02_keepass_dump_extraction.png" width="100%">
</p>

<p align="center">
<b>Figure 02 — KeePass memory parser identifying a candidate master password representation from the recovered process dump.</b>
</p>

---

## Why Candidate Passwords May Be Incomplete

Memory artifacts are rarely perfect.

Possible reasons include:

- Memory overwritten.
- Character encoding loss.
- Heap corruption.
- Non-printable Unicode values.
- Parser uncertainty.

Instead of failing completely, extraction tools may substitute an unknown placeholder.

Example:

```text
●NoWaYIcanF0rGetThis123
```

The placeholder represents an unresolved character.

---

## Forensic Interpretation

The candidate is **evidence**, not proof.

A DFIR workflow treats incomplete strings as hypotheses that require validation against another artifact.

This prevents investigators from assuming recovered strings are automatically correct.

---

# Understanding KeePass Internals

> Understanding KeePass architecture explains why the attacker steals **both** the database and the process dump.

---

## What is KeePass?

KeePass is an open-source password manager that stores credentials inside an encrypted vault.

Common stored secrets include:

- Website passwords.
- SSH keys.
- Database credentials.
- API tokens.
- Secure notes.
- Personal authentication secrets.

---

## KeePass Database Structure

### `.kdbx` File

The database is an encrypted container.

It contains:

| Component | Purpose |
|-----------|---------|
| Header | Version information |
| Cipher parameters | Encryption metadata |
| KDF parameters | Key derivation settings |
| Encrypted payload | Protected vault contents |
| XML database | Credential entries after decryption |

---

## Encryption Overview

KeePass typically uses:

- AES encryption.
- ChaCha20 (newer versions).
- Argon2 or AES-KDF.
- Composite master key derivation.

---

## Composite Key Concept

Vault access may require multiple components.

| Component | Optional |
|-----------|----------|
| Master Password | Required |
| Key File | Optional |
| Windows User Account | Optional |
| Hardware Token | Optional |

This room uses the master password workflow.

---

## Why the Database Alone Isn't Enough

An encrypted database without its key remains computationally expensive to decrypt.

Attackers therefore seek runtime memory because it may contain:

- Master password.
- Derived encryption keys.
- Cached secrets.

This dramatically reduces attack complexity.

---

# Phase 7 — Credential Recovery Methodology

> **Objective:** Validate candidate master password using cryptographic verification instead of manual guessing.

---

## Candidate Reconstruction

Unknown characters are reconstructed using bounded candidate generation.

### Investigation Principle

Instead of brute-forcing the entire password space:

- Keep recovered characters fixed.
- Replace unresolved positions only.
- Validate candidates efficiently.

---

## Candidate Generation Script

```python
import string

base = "[REDACTED]NoWaYIcanF0rGetThis123"

characters = (
    string.ascii_letters +
    string.digits +
    string.punctuation
)

with open("candidates.txt","w") as f:
    for c in characters:
        f.write(base.replace("[REDACTED]", c, 1) + "\n")
```

This produces a small, targeted candidate list.

---

## Why This Works

The parser recovered nearly the entire password.

Instead of searching billions of possibilities:

- Search approximately 90–100 candidate characters.
- Validate each cryptographically.
- Recover the correct password quickly.

---

## Password Validation Pipeline

```text
Candidate Password
        │
        ▼
Generate Wordlist
        │
        ▼
Convert KeePass DB
        │
        ▼
John the Ripper Validation
        │
        ▼
Correct Password Identified
```

---

## Convert KeePass Database

John the Ripper uses a KeePass hash format.

```bash
keepass2john Database1337.kdbx > db.hash
```

---

## Validate Candidates

```bash
john --wordlist=candidates.txt db.hash
```

---

## Display Successful Password

```bash
john --show db.hash
```

### Portfolio Safety

The recovered password is intentionally omitted.

```text
Master Password: [REDACTED]
```

---

## Why John the Ripper?

John validates candidates **cryptographically**.

Advantages:

- Correct KDF implementation.
- Supports KeePass hashes.
- Confirms password correctness.
- Prevents false positives.

---

# Phase 8 — KeePass Vault Validation

> **Objective:** Authenticate into the recovered KeePass database and verify the investigation without exposing sensitive data.

---

## Installing KeePassXC CLI

```bash
sudo apt install keepassxc
```

KeePassXC provides a secure CLI for vault inspection.

---

## Listing Vault Entries

```bash
keepassxc-cli ls Database1337.kdbx
```

This enumerates entry names without revealing credentials.

---

## Viewing Target Entry

```bash
keepassxc-cli show Database1337.kdbx "You win!"
```

---

## Evidence Validation

<p align="center">
<img src="../docs/assets/03_keepass_vault_recovery.png" width="90%">
</p>

<p align="center">
<b>Figure 03 — KeePassXC CLI successfully authenticating into the recovered vault.</b>
</p>

The vault opens successfully using the validated credential.

---

## Public Portfolio Redactions

The following values remain hidden.

| Sensitive Item | Public Version |
|----------------|---------------|
| Challenge Flag | `THM{FLAG_REDACTED}` |
| Master Password | `[REDACTED]` |
| Vault UUID | `[REDACTED]` |
| Stored Credentials | `[REDACTED]` |

This preserves educational value while preventing direct answer leakage.

---

## Validation Checklist

| Validation Step | Status |
|-----------------|--------|
| Database recovered | ✅ |
| Memory dump recovered | ✅ |
| XOR reversed | ✅ |
| Base64 decoded | ✅ |
| Candidate generated | ✅ |
| Password validated | ✅ |
| Vault authenticated | ✅ |
| Target entry confirmed | ✅ |

---

# Investigation Findings

## Primary Findings

### Network Findings

- HTTP delivered PowerShell payload.
- Separate TCP exfiltration channels identified.
- Encoded payloads reconstructed successfully.

### Host Findings

- KeePass process was active.
- Memory dump created during runtime.
- KeePass database accessed.

### Credential Findings

- Password candidate recovered from memory.
- Candidate validated cryptographically.
- Vault opened successfully.

---

## Evidence Correlation Summary

| Evidence Source | Investigation Outcome |
|-----------------|----------------------|
| HTTP Traffic | Payload recovered |
| PowerShell Script | Exfiltration workflow identified |
| TCP Stream 1337 | Process dump recovered |
| TCP Stream 1338 | KeePass database recovered |
| Memory Dump | Candidate password recovered |
| KeePass Database | Credential validation completed |

---

# Digital Forensics Lessons Learned

## Key Investigation Principles

### 1. Network Traffic Tells the Story

PCAP analysis revealed:

- Initial compromise workflow.
- Exfiltration channels.
- Artifact locations.
- Encoding methodology.

### 2. Malware Explains Evidence

Static PowerShell analysis identified:

- File paths.
- XOR keys.
- Destination ports.
- Recovery methodology.

### 3. Memory Complements Disk Evidence

The encrypted database alone was insufficient.

The memory dump provided the missing runtime evidence required for successful authentication.

### 4. Validation Is Critical

Every recovered artifact was validated before proceeding.

This prevents investigation errors caused by corrupted streams or incorrect decoding.

---

---

# Phase 9 — Indicators of Compromise (IOC Analysis)

> **Objective:** Extract and document observable indicators discovered during the forensic investigation that could assist Security Operations Center (SOC) teams during incident response.

Unlike traditional malware investigations where Indicators of Compromise (IOCs) may include malicious domains or hashes, this laboratory focuses on **behavioral** and **network-based indicators** extracted from captured evidence.

---

## IOC Summary

| IOC Category | Indicator | Investigation Value |
|--------------|----------|--------------------|
| Protocol | HTTP | Initial payload delivery |
| File Type | `.ps1` | Malicious PowerShell payload |
| TCP Port | `1337` | KeePass process dump exfiltration |
| TCP Port | `1338` | KeePass database exfiltration |
| TCP Port | `1339` | PowerShell payload hosting |
| File | `Database1337.kdbx` | Encrypted password vault |
| File | `keepprocess.dmp` | Windows process memory dump |
| Encoding | Base64 | Binary payload transport |
| Obfuscation | XOR (`0x41`) | Process dump encoding |
| Obfuscation | XOR (`0x42`) | KeePass database encoding |

---

## Network Indicators

### Suspicious Communication Pattern

| Observation | Security Relevance |
|-------------|-------------------|
| PowerShell downloads `.ps1` over HTTP. | Initial execution stage. |
| Outbound TCP communication immediately follows. | Possible exfiltration activity. |
| Multiple uncommon TCP ports. | Data staging behavior. |
| Base64 payload inside TCP stream. | Encoded binary transmission. |

---

## Host-Based Indicators

### Files Created

| Artifact | Purpose |
|----------|---------|
| `Database1337.kdbx` | Password manager vault |
| `keepprocess.dmp` | Memory snapshot |
| Temporary raw stream files | Investigator reconstruction artifacts |

---

## Process Indicators

Potential process names observed during analysis include:

```text
KeePass.exe
powershell.exe
```

These become useful hunting pivots in endpoint telemetry.

---

# Phase 10 — MITRE ATT&CK Mapping

> Mapping attacker behavior to MITRE ATT&CK helps defenders understand the techniques demonstrated in the room.

---

## ATT&CK Matrix

| Tactic | Technique | ATT&CK ID |
|--------|-----------|-----------|
| Execution | PowerShell | **T1059.001** |
| Execution | Command & Scripting Interpreter | **T1059** |
| Credential Access | OS Credential Dumping (Process Memory) | **T1003** |
| Credential Access | Credentials from Password Stores | **T1555** |
| Defense Evasion | Obfuscated Files or Information | **T1027** |
| Collection | Data from Local System | **T1005** |
| Collection | Archive Collected Data | **T1560** *(conceptual)* |
| Exfiltration | Exfiltration Over Alternative Protocol | **T1048** |
| Exfiltration | Exfiltration Over Command and Control Channel | **T1041** |

---

## ATT&CK Attack Chain

```text
Execution
   │
   ▼
PowerShell Payload
   │
   ▼
Credential Collection
   │
   ├──────── KeePass Database
   └──────── KeePass Memory Dump
             │
             ▼
Obfuscation
             │
             ▼
Encoded Network Transmission
             │
             ▼
Credential Recovery
```

---

## Defensive Perspective

Mapping techniques allows defenders to build:

- Sigma rules.
- SIEM detections.
- Threat hunting queries.
- ATT&CK Navigator coverage.

---

# Phase 11 — Detection Engineering Opportunities

> **Objective:** Identify opportunities where SOC tooling could detect this activity before credential theft succeeds.

---

## Detection Opportunity Matrix

| Attack Stage | Detection Opportunity |
|--------------|----------------------|
| PowerShell execution | Monitor suspicious PowerShell invocation. |
| HTTP script download | Alert on `.ps1` downloaded via HTTP. |
| Process dump creation | Detect dump generation for KeePass process. |
| Base64 network payload | Detect unusually long Base64 outbound traffic. |
| TCP ports 1337/1338 | Alert on uncommon outbound connections. |
| KeePass process access | Monitor unauthorized memory access. |

---

## Example Sigma Detection Logic (Conceptual)

### Suspicious PowerShell Download

```yaml
title: Suspicious PowerShell Script Download

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

This is an educational example demonstrating behavioral detection logic.

---

## Example Sigma — KeePass Process Dump

```yaml
title: KeePass Process Dump Creation

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

---

## EDR Detection Opportunities

### Microsoft Defender / CrowdStrike / SentinelOne

Potential alerts include:

- PowerShell downloads executable script.
- Process dump created from password manager.
- Encoded outbound TCP payload.
- Suspicious Base64 activity.
- Unusual access to KeePass process memory.

---

## Threat Hunting Ideas

### Hunt for PowerShell Downloads

```kusto
DeviceProcessEvents
| where ProcessCommandLine contains ".ps1"
| where ProcessCommandLine contains "http"
```

---

### Hunt for KeePass Memory Access

```kusto
DeviceEvents
| where InitiatingProcessFileName =~ "powershell.exe"
| where AdditionalFields contains "KeePass"
```

---

### Hunt for Base64 Traffic

Network analytics can identify unusually large outbound Base64 payloads transmitted over uncommon ports.

---

# Phase 12 — Digital Forensics Analysis

> This section summarizes forensic evidence collected throughout the investigation.

---

## Evidence Correlation Matrix

| Evidence | Source | Finding |
|----------|--------|--------|
| HTTP GET request | PCAP | PowerShell payload delivery |
| PowerShell payload | HTTP response | Exfiltration workflow identified |
| TCP Stream 1337 | Packet Capture | KeePass memory dump recovered |
| TCP Stream 1338 | Packet Capture | KeePass database recovered |
| Windows MiniDump | Process Memory | Candidate master password recovered |
| KeePass Database | Recovered Artifact | Vault successfully validated |

---

## Artifact Reconstruction Summary

```text
Network Evidence
      │
      ▼
PowerShell Payload
      │
      ▼
Exfiltration Streams
      │
      ▼
Recovered Binary Artifacts
      │
      ▼
Memory Analysis
      │
      ▼
Credential Validation
```

---

## Investigation Confidence

| Investigation Stage | Confidence |
|---------------------|------------|
| HTTP analysis | High |
| PowerShell behavior | High |
| Stream reconstruction | High |
| XOR reversal | High |
| Base64 decoding | High |
| Artifact validation | High |
| Password validation | High |
| Vault authentication | High |

---

# Phase 13 — Incident Response Playbook

> A SOC analyst responding to this activity could follow a structured workflow similar to the investigation performed in this room.

---

## Detection Phase

1. Receive network alert.
2. Identify suspicious PowerShell traffic.
3. Preserve packet capture.
4. Identify communicating hosts.

---

## Containment Phase

- Isolate affected workstation.
- Preserve volatile memory if possible.
- Prevent additional outbound connections.
- Collect PowerShell logs.

---

## Investigation Phase

- Recover PowerShell payload.
- Reconstruct TCP streams.
- Recover artifacts.
- Validate recovered evidence.
- Correlate host and network events.

---

## Eradication Phase

- Remove malicious PowerShell payload.
- Rotate affected credentials.
- Review KeePass vault integrity.
- Reset exposed passwords.

---

## Recovery Phase

- Restore trusted credential vault.
- Review endpoint telemetry.
- Verify no persistence remains.
- Monitor for repeated outbound behavior.

---

## Lessons for Incident Responders

This challenge emphasizes:

- Preserve evidence before cleanup.
- Validate artifacts before assumptions.
- Correlate multiple evidence sources.
- Treat encrypted storage and runtime memory differently.

---

# Phase 14 — Blue Team Recommendations

---

## Endpoint Hardening

### PowerShell Controls

Recommendations include:

- Enable PowerShell logging.
- Enable Script Block Logging.
- Enable Module Logging.
- Restrict unsigned scripts where appropriate.

---

## Memory Protection

- Monitor process dump creation.
- Restrict debugging privileges.
- Alert on password manager memory access.
- Protect sensitive applications using endpoint controls.

---

## Network Monitoring

SOC teams should monitor:

- Outbound Base64 payloads.
- HTTP-delivered PowerShell scripts.
- Long-lived TCP sessions on unusual ports.
- Large outbound encoded payloads.

---

## Credential Protection

Recommendations include:

- Strong KeePass master password.
- Hardware-backed MFA where possible.
- Minimize vault unlock duration.
- Lock workstation when away.
- Review clipboard settings.

---

## Logging Recommendations

| Source | Importance |
|--------|------------|
| PowerShell Operational Logs | High |
| Windows Event Logs | High |
| Sysmon Process Events | High |
| Network Flow Logs | High |
| EDR Telemetry | High |

---

# Phase 15 — Investigation Timeline Summary

| Timeline | Investigation Activity |
|----------|-----------------------|
| Stage 1 | Inspect PCAP. |
| Stage 2 | Discover PowerShell payload. |
| Stage 3 | Identify exfiltration ports. |
| Stage 4 | Recover TCP streams. |
| Stage 5 | Decode Base64 payloads. |
| Stage 6 | Reverse XOR obfuscation. |
| Stage 7 | Validate KeePass artifacts. |
| Stage 8 | Analyze Windows memory dump. |
| Stage 9 | Recover candidate credential. |
| Stage 10 | Authenticate KeePass vault. |
| Stage 11 | Validate challenge objective (redacted). |

---

# Lessons Learned

## Technical Lessons

### Network Forensics

- HTTP traffic often reveals initial payload delivery.
- TCP stream reconstruction is more reliable than packet-by-packet extraction.
- Encoded payloads require reconstruction before analysis.

### Malware Analysis

- Reading attacker scripts often reveals recovery methodology.
- Obfuscation does not necessarily imply encryption.
- XOR and Base64 are common lightweight transformations.

### Memory Forensics

- Runtime memory complements disk artifacts.
- UTF-16LE extraction is important on Windows.
- Candidate secrets require validation.

### Credential Security

- Password managers protect secrets at rest.
- Runtime memory remains a valuable attack surface.
- Memory dumps deserve the same protection as encrypted databases.

---

## Defensive Lessons

SOC analysts should correlate:

- Endpoint telemetry.
- PowerShell logs.
- Network traffic.
- Memory artifacts.
- Credential store activity.

Correlation provides stronger evidence than any single source.

---

# References

The following resources provide additional background for the techniques demonstrated in this investigation.

## Official Documentation

- Microsoft PowerShell Documentation.
- Microsoft Windows MiniDump Documentation.
- Wireshark User Guide.
- TShark Documentation.
- KeePass Documentation.
- KeePassXC Documentation.
- MITRE ATT&CK Framework.

## Educational References

- TryHackMe Digital Forensics Learning Path.
- Windows Incident Response documentation.
- Memory Forensics learning resources.

---

# Responsible Disclosure

This repository intentionally **redacts sensitive challenge artifacts**.

### Redacted Items

| Item | Public Repository |
|------|-------------------|
| TryHackMe Flag | `THM{FLAG_REDACTED}` |
| KeePass Master Password | `[REDACTED]` |
| Vault UUID | `[REDACTED]` |
| Credential Values | `[REDACTED]` |

The documentation demonstrates the investigation methodology without publishing challenge answers.

---

# Conclusion

## Final Investigation Summary

**Extracted** demonstrates a realistic digital forensic investigation where multiple evidence sources must be correlated to reconstruct attacker behavior.

The investigation began with a **packet capture**, progressed through **PowerShell malware analysis**, recovered **encoded artifacts** using TCP stream reconstruction, reversed **Base64 and XOR transformations**, analyzed **Windows process memory**, and successfully validated an encrypted **KeePass credential vault**.

Unlike exploitation-focused Capture-The-Flag rooms, this challenge emphasizes the investigative mindset used by Security Operations Center (SOC) analysts and Digital Forensics & Incident Response (DFIR) practitioners.

### Key Takeaways

- Network traffic can reveal attacker tooling and exfiltration methods.
- Malware analysis often explains how evidence should be reconstructed.
- Memory artifacts provide context unavailable from disk alone.
- Validation is an essential forensic principle before drawing conclusions.
- Evidence correlation is the foundation of professional incident response.

This walkthrough serves as a **portfolio-quality DFIR report** documenting an end-to-end forensic investigation inside an authorized TryHackMe laboratory while preserving responsible disclosure by redacting sensitive challenge secrets.

---

<div align="center">

## 🛡️ Investigation Complete

**Extracted — TryHackMe Digital Forensics Walkthrough**

*Network Forensics • Memory Analysis • Incident Response • Credential Recovery*

**Author:** Anurag

⭐ *Built as part of a professional Cybersecurity Portfolio.*

</div>
