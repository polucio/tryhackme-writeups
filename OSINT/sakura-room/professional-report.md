
# OSINT ANALYST REPORT  
**TryHackMe Sakura Room — Attacker Profiling (OSINT)**  
**Case Number:** THM-SAKURA-001  
**Analyst:** Porscha Lucio  
**Date:** 2025-11-22  
**Classification:** INTERNAL — TRAINING USE ONLY  

---

## **1. Executive Summary**

This report documents an open-source intelligence (OSINT) investigation conducted as part of the TryHackMe *Sakura Room* training module.  
The scenario presents a threat actor who publicly taunts investigators, leaving a trail across social media, developer platforms, dark‑web paste sites, WiFi metadata sources, and blockchain transactions.

The goal of this assessment was to:

- **Track** the attacker’s online presence  
- **Identify** infrastructure and operational metadata  
- **Correlate** OSINT artifacts to determine their approximate home location  
- **Document** findings in a professional, analyst‑ready format  

All findings are based on simulated artifacts provided by TryHackMe for educational purposes.

---

## **2. Scope & Limitations**

### **Scope**
This investigation covers OSINT analysis only.  
No intrusive methods, system access, social engineering, or operational engagement were performed.

### **Limitations**
- WiFi geolocation (Wigle.net) reflects **historical** BSSID data from the training environment.  
- Dark‑web paste sites referenced in the challenge are offline; reconstruction relied on screenshots and the MD5 hash provided.  
- Cryptocurrency activity reflects publicly available blockchain records.  
- All artifacts are fictional or simulated unless otherwise stated by TryHackMe.

---

## **3. Objectives**

1. Identify attacker‑controlled accounts and infrastructure  
2. Recover WiFi identifiers (SSID/BSSID) from the attacker’s paste history  
3. Trace publicly visible cryptocurrency transactions  
4. Perform geolocation using satellite imagery provided  
5. Correlate all artifacts to determine the attacker’s likely home location  

---

## **4. Methodology**

### **4.1 Social Media Pivoting (Twitter/X)**

- The attacker taunts investigators using the account:  
  `https://x.com/SakuraLoverAiko`
- Posts reference **DeepPaste**, a dark‑web paste service where the attacker stores WiFi credentials.  
- Additional posts include a **Washington D.C. cherry blossom image**, used later for airport geolocation.

---

### **4.2 Dark‑Web Paste Reconstruction**

The attacker mentions hiding WiFi credentials on a dark‑web service called **DeepPaste**.

The TryHackMe room provides a partial onion domain + the attacker’s MD5 hash:

```
MD5: 0a5c6e136a98a60b8a21643ce8c15a74
```

Reconstructed onion URL:

```
http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74
```

Paste contents included:

- **SSID:** `DK1F-G`  
- Multiple access points labeled “Regular WiFi and Passwords”

---

### **4.3 WiFi Metadata Correlation (Wigle.net)**

Using SSID `DK1F-G`, an advanced search on **Wigle.net** returned the historical BSSID:

```
BSSID: 84:AF:EC:34:FC:F8
```

Location correlation placed the AP near:

- **Louisville, Colorado**
- Superior / Boulder region  
- Coordinates were consistent with known residential WiFi density clusters

---

### **4.4 GitHub Commit Forensics**

The attacker maintains a GitHub repository containing an **ETH mining script**.  
A commit diff exposed a removed stratum line:

```
stratum://0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.Aiko:pswd@eu1.ethermine.org:4444
```

Artifact breakdown:

| Component        | Value |
|------------------|-------|
| **Wallet Address** | `0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef` |
| **Worker ID**      | `Aiko` |
| **Mining Pool**    | `eu1.ethermine.org:4444` |

Blockchain investigation showed:

- Incoming payouts from **Ethermine**  
- Outgoing transfers converting ETH to **USDT (Tether)**  

---

### **4.5 Imagery & Travel OSINT**

Two images were used:

#### **1. Pre‑flight Cherry Blossom Photo**
Contained the Washington Monument → location confirmed as:

```
Ronald Reagan Washington National Airport (DCA)
```

#### **2. In‑flight Satellite Map**
Distinct lake shape identified as:

```
Lake Inawashiro — Fukushima Prefecture, Japan
```

This confirms the attacker was returning home from Japan.

---

## **5. Findings**

| Category | Key Result |
|---------|------------|
| **Attacker’s X Account** | `https://x.com/SakuraLoverAiko` |
| **Real Name Used** | Aiko Abe |
| **WiFi SSID (Home)** | `DK1F-G` |
| **Historical BSSID** | `84:AF:EC:34:FC:F8` |
| **Crypto Wallet Type** | Ethereum |
| **Wallet Address** | `0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef` |
| **Mining Pool** | Ethermine |
| **Currency Exchanged To** | USDT (Tether) |
| **Closest Airport on Departure** | DCA |
| **Lake in Map Screenshot** | Lake Inawashiro |
| **Likely Home Location** | Louisville, Colorado |

---

## **6. Conclusion**

All artifacts—WiFi metadata, social media behavior, GitHub commit exposure, crypto tracing, and travel imagery—correlate strongly to place the attacker’s likely home base in **Louisville, Colorado**.

The combination of OSINT sources demonstrates the importance of:

- Digital hygiene  
- Avoiding credential reuse  
- Avoiding posting sensitive metadata online  

This investigation is representative of real-world cyber threat intelligence workflows.

---

## **7. Appendix**

### Artifact List (Condensed)
- MD5 hash used in DeepPaste reconstruction  
- Satellite imagery of Lake Inawashiro  
- Stratum mining string from GitHub diff  
- BSSID and SSID from Wigle metadata  
- Ethereum wallet transaction logs  

---

## **8. References**

**Video Walkthrough Used for Reinforcement:**  
*TryHackMe Sakura Room Walkthrough Tasks 4‑6*  
OSINT Dojo, 2021‑04‑18  
https://www.youtube.com/watch?v=JUyyfcbf7Nw

---

*End of Report*
