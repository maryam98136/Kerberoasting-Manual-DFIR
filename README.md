# Kerberoasting-Manual-DFIR
# Kerberoasting Attack Investigation: Manual Log Analysis

## 📖 Project Overview
This repository documents a complete **manual forensic investigation** of a Kerberoasting attack against an Active Directory environment.

**Key Distinction:** This investigation was performed **entirely without automated scripts**. I was provided with a raw log file (`.evtx`/`.txt`) and manually correlated events using native Windows Event Viewer/Notepad. This demonstrates core DFIR skills: log parsing, timeline reconstruction, and threat hunting without reliance on SIEM tools.

## 🎯 Attack Summary
- **Initial Compromise:** User `alonzo.spire` (Source IP: `172.17.79.129`).
- **Attack Vector:** Kerberoasting (MITRE T1558.003) - Attacker requested an RC4 ticket for `MSSQLService`.
- **Escalation:** Administrator account logged in shortly after, indicating lateral movement.
- **Failed Move:** Attacker attempted Pass-the-Hash using `FORELA-WKSTN001$` but failed (Event 4771).

## 🔍 Investigation Methodology
1. **Data Triage:** Isolated suspicious Event IDs (4769, 4771, 4624, 4625).
2. **Field Analysis:** Manually checked `TicketEncryptionType` (0x17 = RC4), `SubjectUserName` (excluded `$` machine accounts), and `IpAddress`.
3. **Timeline Correlation:** Mapped all events chronologically to identify the attack chain.
4. **Noise Filtering:** Ruled out benign DC housekeeping events (SYSVOL, IPC$, TGT renewals).

## 📂 Repository Contents
- `/logs` - Raw XML evidence files.
- `/analysis` - My step-by-step investigator notes and thought process.
- `/timeline` - Chronological breakdown of the attack.
- `/reports` - Executive summary and remediation steps.

## 🛡️ Remediation Highlights
- Reset `MSSQLService`, `alonzo.spire`, and `Administrator` passwords.
- Isolated IP `172.17.79.129`.
- Recommended disabling RC4 encryption for Kerberos.

---
*This was a lab exercise conducted for educational purposes. All data is fictional.*
