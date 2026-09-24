# Extracted - Analyst Notes

## Core Chain

`PCAP -> HTTP PowerShell -> TCP/1337 + TCP/1338 -> Base64 -> XOR -> .dmp + .kdbx -> password candidate -> KeePass validation`

## Ports

| Port | Artifact | XOR |
|---:|---|---:|
| 1337 | KeePass process dump | `0x41` |
| 1338 | KeePass database | `0x42` |
| 1339 | PowerShell delivery | n/a |

## Useful Filters

```text
http
tcp.port == 1337
tcp.port == 1338
tcp.port == 1339
```

## Recovery Commands

```bash
tshark -r traffic.pcapng -Y "tcp.port == 1338" -T fields -e tcp.payload | tr -d '\n' > combined_hex_1338.txt
xxd -r -p combined_hex_1338.txt > stream_1338.raw

tshark -r traffic.pcapng -Y "tcp.port == 1337" -T fields -e tcp.payload | tr -d '\n' > combined_hex_1337.txt
xxd -r -p combined_hex_1337.txt > stream_1337.raw

python3 recover.py stream_1338.raw Database1337.kdbx 0x42
python3 recover.py stream_1337.raw keepprocess.dmp 0x41
```

## Artifact Checks

```bash
file Database1337.kdbx
file keepprocess.dmp
strings -el keepprocess.dmp | head -n 50
```

## KeePass Workflow

```bash
keepass2john Database1337.kdbx > db.hash
john --wordlist=candidates.txt db.hash
john --show db.hash

keepassxc-cli ls Database1337.kdbx
keepassxc-cli show Database1337.kdbx "You win!"
```

## Public-Writeup Rule

Never publish the original flag, exact recovered password, or unique vault UUID in the public portfolio. Keep them in private lab notes if needed for challenge verification.
