# 🛡️ VulnHub Machines — My Walkthrough Archive

<p align="center">
  <a href="https://www.vulnhub.com">VulnHub</a> walkthroughs and notes on machines I have solved to learn offensive security.
</p>
<p align="center">
  <img src="https://img.shields.io/badge/machines-2-blue" alt="machines">
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
