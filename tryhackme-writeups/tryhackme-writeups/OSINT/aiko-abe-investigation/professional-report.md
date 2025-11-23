# OSINT Investigation Report: “Aiko Abe” Persona (TryHackMe Case)

## 1. Executive Summary
A simulated ransomware actor operating under multiple “Aiko”‑themed aliases left OSINT artifacts across social media, GitHub, and cryptocurrency infrastructure. The investigation correlated these artifacts to attribute the online persona, identify a cryptocurrency wallet and mining pool, recover home WiFi metadata from a dark‑web paste reference, and infer likely travel and home location.

**Bottom line:** the attacker persona is attributed to **Aiko Abe**, operating the X/Twitter handle **@SakuraLoverAiko**, mining Ethereum via Ethermine, converting proceeds to USDT, and exhibiting location indicators consistent with **Louisville, Colorado (USA)**.

---

## 2. Scope & Objectives
**Scope:** Open‑source artifacts only (no intrusion, no private data access).  
**Objectives:**
1. Identify the attacker’s current social media handle and claimed real‑world name.
2. Reconstruct dark‑web paste reference to recover WiFi SSID metadata.
3. Extract cryptocurrency wallet indicators from GitHub activity.
4. Trace wallet activity and determine mining/exchange behavior.
5. Use imagery and travel clues to infer the attacker’s likely home location.

---

## 3. Sources & Artifacts
- X/Twitter posts from attacker‑controlled account.
- Public GitHub repositories and commit diffs.
- TryHackMe‑provided hint screenshots (training artifacts).
- Wigle.net SSID/BSSID geolocation dataset.
- Etherscan wallet transaction history.
- Public mapping/imagery for geolocation.

---

## 4. Findings

### 4.1 Identity & Social Media Attribution
**Handle:** https://x.com/SakuraLoverAiko  
**Claimed real name:** “Aiko Abe”  
A self‑introductory tweet from @AikoAbe3 links the persona to the name “Aiko Abe.” The account simultaneously taunts OSINT investigators, suggesting deliberate breadcrumb placement.

---

### 4.2 Dark‑Web Paste Reconstruction & WiFi Metadata
The attacker references storing WiFi SSIDs/passwords on “DeepPaste” and posts an MD5 value in an image. THM provides a partial onion template. Combining these yields:

**Onion URL:**  
http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74

The DeepPaste screenshot titled **“Regular WiFi and Passwords”** lists the attacker’s home SSID.

**Home SSID:** DK1F‑G

---

### 4.3 GitHub Commit Forensics → Wallet Discovery
The attacker created repositories related to cryptocurrency mining, notably **ETH**.  
A commit history diff exposed a removed mining configuration line referencing:

- Ethereum wallet
- worker identifier
- pool endpoint

**Recovered stratum line (training artifact):**
```
stratum://0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.Aiko:pswd@eu1.ethermine.org:4444
```

**Wallet type:** Ethereum  
**Wallet address:** 0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef  
**Mining pool:** Ethermine

---

### 4.4 Blockchain Tracing (Etherscan)
Wallet analysis on Etherscan confirms:
- repeated inbound payouts labeled **Ethermine**
- outbound ETH transfers including conversions to **USDT (Tether)**

**Other cryptocurrency exchanged into:** USDT

---

### 4.5 Travel & Geolocation Indicators
Two key imagery artifacts were leveraged:

1) **Pre‑flight photo** featuring cherry blossoms and the Washington Monument  
   → Geolocated to Washington, DC  
   → nearest airport: **DCA (Reagan National Airport)**

2) **In‑flight satellite map** containing a distinctive lake  
   → identified as **Lake Inawashiro** (Fukushima Prefecture, Japan)

Cross‑referencing the attacker’s WiFi geolocation from SSID/BSSID analysis supports return travel toward Colorado.

**Likely home city:** Louisville, Colorado

---

## 5. Methodology (High‑Level)
1. Username pivoting → social media attribution.
2. Hash + template reconstruction → onion reference recovery.
3. GitHub commit review → wallet extraction.
4. Blockchain OSINT → pool + exchange behavior.
5. Imagery geolocation → airport and travel route inference.
6. WiFi geolocation via Wigle → home‑region triangulation.

---

## 6. Limitations
- Dark‑web site was unreachable live; relied on training screenshots.
- Wigle records evolve; historical BSSID values may differ from current results.
- Persona attribution is based on self‑claimed identity in public posts (training context).

---

## 7. Conclusion
The OSINT trail coherently attributes the simulated attacker persona to **Aiko Abe**, ties their infrastructure to a specific ETH wallet and Ethermine pool, and places their likely home environment near **Louisville, Colorado**. The case demonstrates effective multi‑source OSINT synthesis across identity, WiFi metadata, crypto tracing, and imagery analysis.

---

## 8. Reference
OSINT reinforcement video used during investigation:  
https://www.youtube.com/watch?v=JUyyfcbf7Nw

---

## 9. Ethics Statement
This report is for defensive training and educational purposes.  
All “attacker” data is part of a fictional TryHackMe scenario.