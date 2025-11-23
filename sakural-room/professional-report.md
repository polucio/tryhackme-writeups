OSINT ANALYST REPORT

TryHackMe Sakura Room — Attacker Profiling (OSINT)
Case Number: THM-SAKURA-001
Analyst: Porscha Lucio
Date: 2025-11-22
Classification: INTERNAL — TRAINING USE ONLY

1. Executive Summary

This report documents an open-source intelligence (OSINT) investigation conducted as part of the TryHackMe Sakura Room training module.
The scenario presents a threat actor who publicly taunts investigators, leaving a trail across social media, developer platforms, dark-web paste sites, WiFi metadata sources, and blockchain transactions.

The goal of this assessment was to:

Track the attacker’s online presence

Identify infrastructure and operational metadata

Correlate OSINT artifacts to determine their approximate home location

Document findings in a professional, analyst-ready format

All findings are based on simulated artifacts provided by TryHackMe for educational purposes.

2. Scope & Limitations

Scope:
This investigation covers OSINT analysis only. No intrusive methods, system access, social engineering, or operational engagement were performed.

Limitations:

WiFi geolocation (Wigle.net) reflects historical BSSID data from the training environment

Dark-web paste sites referenced in the challenge are now offline; reconstruction relied on the provided screenshot and MD5 hash

Cryptocurrency transactions were reviewed using publicly available blockchain data

All artifacts are fictional or simulated unless otherwise stated by TryHackMe.

3. Objectives

Identify attacker’s online alias

Locate their associated dark-web paste

Extract WiFi SSID/BSSID

Investigate GitHub commits for exposed credentials

Trace cryptocurrency wallet activity

Determine the attacker’s likely location during travel

Produce a professional intelligence report

4. Findings & Analysis
4.1 Social Media Pivoting — X/Twitter

A taunting message from the attacker surfaced at the start of the challenge.

Recovered profile:

Username: @SakuraLoverAiko
Platform: X (Twitter)
Profile Name: Aiko Abe
Join Date: January 2021

The attacker posted multiple operational breadcrumbs including:

WiFi credential paste references

A screenshot containing an MD5 hash

Travel photos revealing their location

A satellite map during flight

These served as pivot points for deeper analysis.

4.2 Dark-Web Paste Discovery (DeepPaste)

The attacker stated:

“Anyone who wants them will have to do a real DEEP search to find where I PASTEd them.”

This indicates a dark-web paste site — DeepPaste.

A provided screenshot contained a paste titled:

“Results for 0a5c6e136a98a60b8a21643ce8c15a74”

Using the template URL shown in the TryHackMe room:

http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=<HASH>


Final reconstructed onion link:

http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3ll7hzix7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74


Extracted Data From Paste:

Home SSID: DK1F-G

Multiple non-home access points also listed

This SSID becomes the pivot for geolocation.

4.3 WiFi Metadata Correlation (Wigle)

Using wigle.net and the recovered SSID DK1F-G:

Historical BSSID from the challenge’s answer key:
84:AF:EC:34:FC:F8

This BSSID mapped to the Louisville / Superior / Boulder, Colorado region.

This is the attacker’s likely residential area.

4.4 GitHub Commit Forensics

The attacker maintained a GitHub repository containing mining software (ETH-related).

A commit diff revealed a removed mining configuration line:

stratum://0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.Aiko:pswd@eu1.ethermine.org:4444


A breakdown of this artifact follows:

Recovered Cryptocurrency Artifact
Field	Value
Wallet Address	0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef
Worker ID	Aiko
Mining Pool Endpoint	eu1.ethermine.org:4444
Protocol	Stratum
4.5 Blockchain Analysis (Etherscan)

Reviewing the wallet on etherscan.io identified:

Multiple incoming deposits labeled “Ethermine”

Outbound transactions converting ETH → USDT (Tether)

Wallet shows miner-style micro-transactions consistent with GPU/CPU mining

Cryptocurrency identified:

Primary: Ethereum

Exchanged to: USDT (Tether)

4.6 Travel & Geolocation OSINT

Two images were key:

A. Pre-flight Photo

The attacker posted a picture containing cherry blossoms and a tall obelisk-like structure.

This was visually matched to:

The Washington Monument — Washington, DC

Nearest airport:
Ronald Reagan Washington National Airport (DCA)

B. In-Flight Map

The satellite map shows a distinct large lake.

Identified as:
Lake Inawashiro (Fukushima Prefecture, Japan)

This confirms the attacker was flying toward their home region in the US after time abroad.

5. Consolidated Summary of Findings
Category	Finding
Attacker Alias	@SakuraLoverAiko
Real Name (online)	Aiko Abe
Dark-Web Paste Site	DeepPaste
Reconstructed Onion Link	http://depaste.../show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74
Home SSID	DK1F-G
Historical BSSID	84:AF:EC:34:FC:F8
Crypto Wallet	Ethereum — 0xa10239…B6ef
Mining Pool	Ethermine
Final Destination Lake on Map	Lake Inawashiro
Departure Airport	DCA
Likely Home City	Louisville, Colorado
6. Tools Used

X (Twitter)

GitHub commit history

Tor Browser (dark-web link reconstruction)

Wigle.net

Google Maps / Earth imagery

Etherscan.io

CyberChef (hash reference checking)

7. Analyst Notes

This investigation mirrors real-world OSINT workflows:

Social media identity pivoting

Dark-web artifact reconstruction

Blockchain transaction tracing

Wireless metadata geolocation

Commit forensics via GitHub

The challenge reinforces skills aligned with threat intelligence, SOC triage, DFIR enrichment, and GRC documentation standards.

8. References

[1] OSINT Dojo. TryHackMe Sakura Room Walkthrough (Tasks 4–6).
April 18, 2021.
https://www.youtube.com/watch?v=JUyyfcbf7Nw

[2] TryHackMe — Sakura Room.
https://tryhackme.com/room/sakura

[3] Etherscan.io Blockchain Explorer
https://etherscan.io

[4] Wigle Wireless Geolocation Platform
https://wigle.net
