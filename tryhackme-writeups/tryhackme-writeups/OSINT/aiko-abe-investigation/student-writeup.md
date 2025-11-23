# OSINT Case Study: Unmasking “Aiko Abe” (TryHackMe) — Student Write‑Up

## Challenge Overview
In this TryHackMe OSINT challenge, a ransomware actor taunts investigators and leaves breadcrumbs across social media, GitHub, a dark‑web paste site, WiFi metadata, and cryptocurrency activity.  
Goal: follow those trails to identify the attacker’s identity, infrastructure, and likely home location.

---

## Key Findings
- **Twitter/X account:** https://x.com/SakuraLoverAiko  
- **Real name used online:** Aiko Abe  
- **Dark‑web paste service:** DeepPaste  
- **Reconstructed onion URL:**  
  http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74  
- **Home SSID:** DK1F‑G  
- **Home BSSID (historical value used by the room):** 84:AF:EC:34:FC:F8  
- **Crypto wallet type:** Ethereum  
- **Ethereum wallet:** 0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef  
- **Mining pool:** Ethermine  
- **Converted/exchanged to:** USDT (Tether)  
- **Travel clue lake:** Lake Inawashiro  
- **Closest airport before departure:** DCA (Reagan National Airport)  
- **Likely home city:** Louisville, Colorado  

---

## Walkthrough

### 1. Social Media Pivoting
We started with the attacker’s username and taunt.  
Searching the name led to an active X profile:

- https://x.com/SakuraLoverAiko

On this account, the attacker references a “deep” paste site and posts images that later help with location tracking.

---

### 2. Dark‑Web Paste Reconstruction
The attacker hints that their WiFi SSIDs/passwords were saved on **DeepPaste**.

Because the onion site is offline, THM provides:
- a **partial onion URL template**
- a **tweet containing an MD5 value**

We combine them into a full link:

http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74

The DeepPaste screenshot shows a table called **“Regular WiFi and Passwords”**, including the attacker’s home SSID:

- **DK1F‑G**

---

### 3. WiFi Metadata & Wigle
We used the SSID **DK1F‑G** in **Wigle.net Advanced Search** to identify:
- the BSSID of the access point
- its approximate location

Because Wigle updates over time, the live results may differ.  
The historical BSSID value required by the room is:

- **84:AF:EC:34:FC:F8**

This BSSID maps to the Boulder/Superior/Louisville area in Colorado.

---

### 4. GitHub Commit Forensics & Crypto Tracing
The attacker’s GitHub activity includes a repository named **ETH**, created on Jan 23, 2021.

In a commit diff, a deleted mining command reveals a stratum string:

```
stratum://0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.Aiko:pswd@eu1.ethermine.org:4444
```

This includes:
- **ETH wallet**
- **worker name**
- **Ethermine pool endpoint**

Using **Etherscan**, we confirmed:
- multiple inbound mining payouts labeled **Ethermine**
- outbound transactions swapping ETH into **USDT (Tether)**

---

### 5. Travel & Imagery OSINT
The attacker posted:
1) A cherry‑blossom photo with a tall slender monument  
   → identified as **Washington Monument, DC**  
   → closest airport: **DCA**

2) A flight‑map satellite image  
   → lake identified as **Lake Inawashiro** (Fukushima Prefecture)

These clues connect their travel route back toward Colorado.

---

## Tools Used
- Twitter/X  
- GitHub commit history  
- THM hint images  
- Wigle.net (WiFi geolocation)  
- Etherscan (blockchain tracing)  
- Google Maps / Earth  
- CyberChef (hash handling / reconstruction)

---

## Reference Video
I referenced this walkthrough to reinforce OSINT methods:  
https://www.youtube.com/watch?v=JUyyfcbf7Nw

---

## Notes
This write‑up is for educational use only.  
All identities and artifacts are part of a simulated OSINT training scenario.