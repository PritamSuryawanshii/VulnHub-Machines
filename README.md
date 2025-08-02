# 🛡️ VulnHub Machines — My Walkthrough Archive

<p align="center">
  <a href="https://www.vulnhub.com">VulnHub</a> walkthroughs and notes on machines I have solved to learn offensive security.
</p>
<p align="center">
  <img src="https://img.shields.io/badge/machines-4-blue" alt="machines">
  <img src="https://img.shields.io/badge/status-active-brightgreen" alt="status">
</p>

---

🔍 **Why this repo?**

This repo contains my personal collection of VulnHub machine write-ups. Each folder includes my process and solutions to help others learn enumeration, exploitation, and privilege escalation.

---

## 📁 Machines Covered

| #  | Machine           | Difficulty     | Highlights                          | Flags         | Write-up          |
|----|-------------------|----------------|-------------------------------------|---------------|-------------------|
| 01 | Empire: LupinOne  | 🟡 Easy-Medium | SSH Key discovery, Base58, pip privesc | user, root    | Empire-LupinOne   |
| 02 | Corrosion: 2      | 🟠 Medium      | NFS misconfig, CVE-2021-4034 (pwnkit), shadow cracking | user, root | Corrosion-2       |
| 03 | DarkHole V2       | 🔴 Hard        | LFI to RCE chain, custom SUID binary, kernel exploit | user, root | DarkHole-V2       |
| 04 | DC: 7             | 🟡 Easy        | Drupal 7 RCE (Drupalgeddon2), weak creds | user, root    | DC-7              |

---

## 🖋️ Empire: LupinOne (Easy‑Medium)

- **Ports**: 22 (SSH), 80 (HTTP)  
- **Enum**: Hidden dir `/~secret/`, Base58 key  
- **Access**: SSH with cracked private key  
- **Privesc**: `pip` hijacking for root access  
- **Flags**: `user.txt`, `root.txt`

---

## 🖋️ Corrosion: 2 (Medium)

- **Ports**: 21 (FTP), 2049 (NFS), 80 (HTTP)
- **Enum**: Anonymous NFS mount reveals SSH keys and shadow file  
- **Access**: Cracked password from shadow dump via John the Ripper  
- **Privesc**: Exploited `CVE-2021-4034` (Polkit - pwnkit)  
- **Flags**: `user.txt`, `root.txt`

---

## 🖋️ DarkHole V2 (Hard)

- **Ports**: 80 (HTTP), 443 (HTTPS)  
- **Enum**: LFI via vulnerable PHP endpoint, log poisoning  
- **Access**: Reverse shell through crafted access log injection  
- **Privesc**: Analyzed and exploited custom SUID binary, kernel escalation via dirty pipe variant  
- **Flags**: `user.txt`, `root.txt`

---

## 🖋️ DC: 7 (Easy)

- **Ports**: 80 (HTTP), 22 (SSH)  
- **Enum**: Found Drupal CMS, vulnerable to Drupalgeddon2 (CVE-2018-7600)  
- **Access**: Remote code execution through exploit, weak SSH creds  
- **Privesc**: sudo access with no password for user scripts  
- **Flags**: `user.txt`, `root.txt`
