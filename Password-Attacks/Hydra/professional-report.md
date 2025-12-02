# Hydra (TryHackMe) — Professional-Style Report

## 1. Executive Summary

The **Hydra** room on TryHackMe was used as a controlled lab to practice online password brute-forcing techniques.  
Using the `hydra` tool and a common password wordlist, I was able to:

1. Obtain valid web application credentials for the user `molly`.
2. Reuse the same username to brute-force and obtain valid **SSH credentials**.
3. Log into the target host over SSH and recover a second flag.

This exercise demonstrates how weak, commonly used passwords and shared credentials across services can quickly lead to system compromise.

---

## 2. Scope & Objectives

**Target:** TryHackMe lab host (dynamic IP, e.g. `10.66.148.23`)  
**Services in scope:**

- Web login form (`HTTP`)
- SSH (port 22)

**Objectives:**

1. Identify valid login credentials for the web application using `hydra`.
2. Identify valid SSH credentials for the same user.
3. Recover both flags provided by the room.

All testing was performed within the legal, isolated TryHackMe environment.

---

## 3. Methodology

### 3.1. Environment

- **Host:** Kali Linux VM
- **Access:** Connected to TryHackMe network via OpenVPN
- **Tools:**
  - `hydra`
  - `rockyou.txt` password list
  - `ssh` client

### 3.2. VPN Connectivity

Connectivity to the lab environment was established using OpenVPN:

```bash
sudo openvpn ~/Downloads/download.ovpn
```

Once the tunnel was established, the Kali VM could reach the lab host at its assigned IP.

---

## 4. Findings

### 4.1. Finding 1: Weak Web Application Password

**Service:** Web login (HTTP)  
**User:** `molly`  
**Issue:** Weak, commonly used password (`sunshine`) susceptible to brute-force guessing.

#### 4.1.1. Technique

The room documentation indicated:

- The login page is accessible at `/`.
- The request uses an HTTP POST method.
- Failed logins contain the word `incorrect` in the response.

Using this information, the following `hydra` command was executed:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23   http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

#### 4.1.2. Result

Hydra identified valid web credentials:

- **Username:** `molly`
- **Password:** `sunshine`

Using these credentials to authenticate to the web application revealed **Flag 1**.

#### 4.1.3. Impact

Any attacker with network access to this application and a common wordlist could obtain access with minimal effort. This access could potentially expose sensitive data or provide a pivot point for further attack, depending on the application’s functionality and role-based access controls.

#### 4.1.4. Recommendations

- Enforce stronger password policies (length, complexity, no common passwords).
- Implement account lockout or throttling after several failed login attempts.
- Consider Multi‑Factor Authentication (MFA) for web logins where feasible.

---

### 4.2. Finding 2: Weak SSH Password Reuse

**Service:** SSH (port 22)  
**User:** `molly`  
**Issue:** Weak password (`butterfly`) present in standard wordlists and reused for remote shell access.

#### 4.2.1. Technique

After obtaining the username `molly` from the web application, the same username was targeted on SSH using `hydra` and `rockyou.txt`:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23 ssh
```

#### 4.2.2. Result

Hydra discovered valid SSH credentials:

- **Username:** `molly`
- **Password:** `butterfly`

SSH access was then obtained with:

```bash
ssh molly@10.66.148.23
```

Upon login, **Flag 2** was accessible to the `molly` user on the host.

#### 4.2.3. Impact

Compromise of an SSH account typically grants full interactive shell access under that user’s context. In a real environment, the impact could include:

- Access to sensitive files owned by the user.
- Launching additional local privilege escalation attempts.
- Lateral movement to other systems, depending on credentials and SSH configuration.

The reuse of a weak password across multiple services (web & SSH) significantly exacerbates risk.

#### 4.2.4. Recommendations

- Require strong, unique passwords for all SSH accounts.
- Disable password-based SSH authentication where possible; use key-based authentication instead.
- Configure fail2ban or similar mechanisms to detect and block repeated failed SSH login attempts.
- Limit SSH access by:
  - IP allow‑listing (where appropriate),
  - disabling SSH for non-administrative users when not needed.

---

## 5. Defensive Considerations

The Hydra lab reinforces several defensive principles:

1. **Strong Password Policy**
   - Prohibit the use of passwords that appear in common wordlists (such as `rockyou.txt`).
   - Encourage or enforce the use of password managers to reduce password reuse.

2. **Rate Limiting & Lockout**
   - Implement lockout, throttling, or CAPTCHA after a small number of failed login attempts on web applications.
   - Apply similar protections for SSH (e.g., fail2ban, iptables rules, intrusion detection systems).

3. **Multi-Factor Authentication**
   - Introduce MFA for high‑value services (admin panels, remote access).
   - This significantly raises the difficulty of successful brute-force attacks.

4. **Monitoring & Alerting**
   - Log and monitor authentication attempts for abnormal patterns.
   - Configure alerts for repeated failed logins, unusual source IPs, or high‑frequency connections.

5. **Credential Hygiene**
   - Prohibit credential reuse across services (e.g., same username/password for web and SSH).
   - Regularly review and rotate credentials, especially for privileged or service accounts.

---

## 6. Alignment with PenTest+ Objectives

This room and the associated lab work directly align with several **CompTIA PenTest+ (PT0‑003)** objectives, including:

- **1.3 / 1.4:** Conducting password attacks and leveraging common wordlists.
- **2.x:** Identifying and validating vulnerabilities tied to weak authentication.
- **3.x:** Explaining the impact of weak passwords and improper credential management.

The hands-on use of `hydra` against both web and SSH services provides practical experience demonstrating how quickly weak credentials can be exploited in real‑world scenarios.

---

## 7. Conclusion

The TryHackMe **Hydra** room successfully demonstrates:

- How an attacker can use `hydra` with publicly available wordlists to compromise:
  - Web logins
  - SSH services
- How weak, common passwords (`sunshine`, `butterfly`) and credential reuse lead directly to system compromise.
- Why enforcing strong authentication controls, monitoring, and rate limiting is essential for defending modern systems.

This report serves as a reference for both offensive practice and defensive hardening related to password attacks.
