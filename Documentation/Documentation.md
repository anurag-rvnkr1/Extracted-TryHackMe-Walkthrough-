---
title: "Extracted - TryHackMe Walkthrough"
description: "Forensic extraction workflow from PCAP to an encrypted KeePass vault"
---

# Extracted - TryHackMe Walkthrough

> **Lab classification:** Network forensics + malware analysis + credential recovery  
> **Objective:** Recover the challenge artifact chain from packet capture, identify the exfiltration mechanism, reconstruct the stolen KeePass files, recover the vault key material, and validate the final entry without publishing the original flag or live secret.

## 1. Executive Summary

The challenge begins with a packet capture rather than a conventional exposed service. The investigation is therefore driven by evidence in the network traffic.

The practical attack chain observed in the lab is:

**PCAP -> HTTP PowerShell delivery -> payload analysis -> TCP stream extraction -> Base64 decoding -> single-byte XOR reversal -> KeePass database + process dump -> memory analysis -> candidate master password -> KeePass vault validation**

The important lesson is that the encrypted database and the process-memory artifact have different security properties. The `.kdbx` is designed to protect stored secrets at rest, while the process dump can expose material that existed in memory while the password manager was in use.

> **Flag handling:** the original challenge flag is intentionally omitted from this public write-up. The repository documents the method and validation path without publishing the final value.

## 2. Lab Evidence

| Evidence | Role |
|---|---|
| `traffic.pcapng` | Network evidence |
| HTTP response on port 1339 | PowerShell delivery |
| TCP stream on port 1338 | Exfiltrated KeePass database |
| TCP stream on port 1337 | Exfiltrated KeePass process dump |
| `Database1337.kdbx` | Encrypted KeePass vault |
| `keepprocess.dmp` | Windows process memory artifact |

## 3. Initial Triage - Wireshark

Open the PCAP in Wireshark and start with HTTP traffic. A GET request for a `.ps1` resource is the first major pivot.

The User-Agent and response headers indicate PowerShell content being transferred from the lab host.

![PCAP triage](assets/img/01_wireshark_powershell_delivery.png)

**Figure 01 - HTTP PowerShell delivery observed in the packet capture.**

### What this establishes

The capture contains an executable PowerShell stage. The next step is to inspect the response body and understand what the script does with local data.

## 4. PowerShell Payload Analysis

The recovered script performs two related collection actions:

1. It checks for a running KeePass process and creates a process dump when present.
2. It reads a KeePass database file from disk.

The artifacts are transformed before transmission. The payload logic shows:

- process dump bytes XORed with `0x41`
- database bytes XORed with `0x42`
- Base64 encoding
- transmission over separate TCP channels

This explains why raw traffic analysis alone is insufficient. The investigator must reverse the exact transformation sequence.

### Transformation model

```text
KeePass process memory
        |
        v
   XOR with 0x41
        |
        v
    Base64
        |
        v
   TCP/1337

KeePass database
        |
        v
   XOR with 0x42
        |
        v
    Base64
        |
        v
   TCP/1338
```

## 5. Recovering the Exfiltrated Streams

The cleanest Wireshark method is:

**Follow -> TCP Stream -> Raw -> Save**

For repeatable CLI work, `tshark` can isolate the payload for the required port.

```bash
tshark -r traffic.pcapng \
  -Y "tcp.port == 1338" \
  -T fields -e tcp.payload | tr -d '\n' > combined_hex_1338.txt

xxd -r -p combined_hex_1338.txt > stream_1338.raw
```

Repeat the process for port `1337`.

> **Evidence note:** always prefer the reconstructed TCP stream over manually concatenating arbitrary packets when possible. TCP segmentation and retransmission can otherwise create misleading output.

## 6. Decode and Reverse the Transformation

The transformation order in the captured payload is reversed during recovery:

```text
Captured stream
    |
    v
ASCII / Base64 text
    |
    v
Base64 decode
    |
    v
XOR with the known key
    |
    v
Original artifact
```

A compact recovery helper:

```python
#!/usr/bin/env python3
import base64
import sys

def recover(infile: str, outfile: str, xor_key: int) -> None:
    data = open(infile, "rb").read()
    text = data.decode("ascii", errors="ignore")
    decoded = base64.b64decode("".join(text.split()))
    recovered = bytes(b ^ xor_key for b in decoded)

    with open(outfile, "wb") as fh:
        fh.write(recovered)

if __name__ == "__main__":
    if len(sys.argv) != 4:
        raise SystemExit("Usage: recover.py <input> <output> <xor_key>")
    recover(sys.argv[1], sys.argv[2], int(sys.argv[3], 0))
```

Example usage:

```bash
python3 recover.py stream_1338.raw Database1337.kdbx 0x42
python3 recover.py stream_1337.raw keepprocess.dmp 0x41
```

## 7. Artifact Validation

After recovery, validate the artifact type before attempting further analysis.

```bash
file Database1337.kdbx
file keepprocess.dmp

strings keepprocess.dmp | head
strings -el keepprocess.dmp | head
```

The process dump should identify itself as a Windows minidump, while the `.kdbx` file should be recognized as a KeePass database artifact.

## 8. Why the Process Dump Matters

A KeePass database is encrypted at rest. The presence of the `.kdbx` alone does not automatically provide the plaintext credentials.

The process-memory artifact changes the situation because an application that is actively using a secret must process that secret in memory. Memory may therefore contain strings, key material, or temporary representations related to the vault.

This is the central forensic insight of the room:

**the database protects data at rest; the memory dump can expose data in use.**

## 9. Recovering the KeePass Master Password

A purpose-built KeePass dump parser can extract candidate material from the minidump.

Example:

```bash
git clone https://github.com/matro7sh/keepass-dump-masterkey.git
cd keepass-dump-masterkey
python3 poc.py ../keepprocess.dmp
```

![Memory analysis](assets/img/02_keepass_dump_extraction.png)

**Figure 02 - KeePass process dump analysis producing a candidate password representation.**

In this challenge, the parser output contains a non-renderable placeholder for one character. Rather than treating the result as the final answer, the correct workflow is to model the uncertainty and validate candidates against the recovered vault.

## 10. Candidate Generation and Validation

The placeholder can be substituted with a bounded character set and tested against a KeePass hash.

```python
import string

base = "[REDACTED]NoWaYIcanF0rGetThis123"
chars = string.ascii_letters + string.digits + string.punctuation

with open("candidates.txt", "w") as f:
    for c in chars:
        f.write(base.replace("[REDACTED]", c, 1) + "\n")
```

Then:

```bash
keepass2john Database1337.kdbx > db.hash
john --wordlist=candidates.txt db.hash
john --show db.hash
```

Only a candidate that successfully unlocks the vault should be considered validated.

> The exact working password is intentionally redacted from this public documentation.

## 11. KeePass Vault Validation

Once the correct credential is established, use `keepassxc-cli` to inspect the database without publishing the final secret.

```bash
sudo apt install keepassxc
keepassxc-cli ls Database1337.kdbx
keepassxc-cli show Database1337.kdbx "You win!"
```

![Vault validation](assets/img/03_keepass_vault_recovery.png)

**Figure 03 - KeePassXC CLI confirming access to the recovered vault. Sensitive output is retained only as evidence and redacted in this portfolio version.**

The target entry is sufficient to validate end-to-end recovery.

## 12. Investigation Timeline

| Phase | Action | Evidence |
|---|---|---|
| 01 | Inspect PCAP | HTTP + TCP streams |
| 02 | Identify payload | PowerShell `.ps1` |
| 03 | Read transformation logic | XOR + Base64 |
| 04 | Extract TCP/1337 | Process dump stream |
| 05 | Extract TCP/1338 | KeePass database stream |
| 06 | Reverse encoding | Base64 decode + XOR |
| 07 | Parse memory | Candidate master password |
| 08 | Generate candidates | Bounded substitution |
| 09 | Validate | KeePass hash / vault unlock |
| 10 | Confirm target entry | End-to-end evidence |

## 13. Key Skills Demonstrated

### Network forensics
- HTTP request/response inspection
- TCP stream reconstruction
- Port-based traffic filtering
- Raw payload extraction

### Malware / script analysis
- PowerShell behavior tracing
- Identifying local file access
- Reconstructing encoding and obfuscation logic
- Mapping code behavior to network artifacts

### Credential and memory forensics
- Windows minidump inspection
- UTF-16LE string analysis
- Candidate secret recovery
- Validation against an encrypted password store

### Tooling
- Wireshark
- tshark
- Python 3
- `xxd`
- KeePass dump tooling
- John the Ripper
- KeePassXC

## 14. Common Pitfalls

**Saving the wrong stream:** use the stream carrying application data, not unrelated ACK-only traffic.

**Forgetting the transformation order:** Base64 and XOR must be reversed in the opposite order from the payload's encoding pipeline.

**Trusting a partially recovered password:** a masked or corrupted character is a hypothesis, not proof.

**Opening the database too early:** first validate the recovered artifact and then test credentials against the actual `.kdbx`.

## 15. Defensive Takeaways

The lab demonstrates several controls that reduce the impact of this attack pattern:

- Restrict PowerShell execution and script delivery where appropriate.
- Monitor unusual outbound connections from workstation processes.
- Alert on access to password-manager process memory by unexpected tooling.
- Protect sensitive applications with endpoint controls that limit dump creation.
- Treat credential stores and memory artifacts as separate forensic data sources.
- Review outbound data paths for encoded payloads that appear shortly after sensitive file access.

## 16. Conclusion

This room is fundamentally an exercise in **evidence correlation**. The solution is not a single exploit; it is a chain:

**network evidence -> payload understanding -> artifact reconstruction -> memory analysis -> credential validation -> vault inspection**

That workflow is broadly transferable to incident response and digital forensics.

---

### Flag

`THM{FLAG_REDACTED}`

*The final flag value is deliberately hidden in the public portfolio version to reduce direct answer leakage and keep the write-up focused on methodology.*
