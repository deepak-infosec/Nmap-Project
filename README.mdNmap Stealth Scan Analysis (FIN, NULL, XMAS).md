# Nmap Stealth Scan Analysis (FIN, NULL, XMAS)

## Overview

This project demonstrates TCP stealth scanning techniques using Nmap and validates results through packet analysis with Wireshark.

Focus:

* XMAS scan (-sX)
* NULL scan (-sN)
* FIN scan (-sF) [theory]

---

## Tools

* Nmap
* Wireshark
* Kali Linux

---

## XMAS Scan (-sX)

### Command

```bash id="c1"
nmap -sX <target_ip> -p 8080 --reason
```

### Closed Port (8080)

```text id="c2"
Result: closed
Reason: reset (RST)

Packet Flow:
Attacker → Target : FIN, PSH, URG
Target → Attacker : RST, ACK
```

---

### Command

```bash id="c3"
nmap -sX <target_ip> -p 22 --reason
```

### Open Port (22)

```text id="c4"
Result: open|filtered
Reason: no-response

Packet Flow:
Attacker → Target : FIN, PSH, URG
Target → Attacker : (no response)
```

---

## NULL Scan (-sN)

### Command

```bash id="c5"
nmap -sN <target_ip> -p 22 --reason
```

### Open Port (22)

```text id="c6"
Result: open|filtered
Reason: no-response

Packet Flow:
Attacker → Target : NO FLAGS
Target → Attacker : (no response)
```

---

## FIN Scan (-sF)

### Behavior

```text id="c7"
FIN packet sent

Closed port → RST
Open port → No response
```

---

## Key Concept

```text id="c8"
Closed Port  → RST response
Open Port    → No response
```

---

## Scan Comparison

```text id="c9"
Scan   | Flags         | Closed | Open
---------------------------------------
SYN    | SYN           | RST    | SYN-ACK
FIN    | FIN           | RST    | No response
NULL   | NONE          | RST    | No response
XMAS   | FIN,PSH,URG   | RST    | No response
```

---

## Screenshots

Add your captures here:

* Wireshark (XMAS closed)
* Wireshark (XMAS open)
* Wireshark (NULL open)
* Nmap outputs

---

## Notes

* XMAS/NULL/FIN scans rely on RFC behavior
* Cannot distinguish open vs filtered ports
* Less reliable against modern systems and Windows

---

## Conclusion

Stealth scans allow identification of port states without completing a full TCP handshake, but results must be verified using packet-level analysis.

This project demonstrates that behavior using real scan data.
