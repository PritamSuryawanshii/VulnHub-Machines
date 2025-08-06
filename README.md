# 🛡️ VulnHub Machines — My Walkthrough Archive

---

🔍 **Why this repo?**

This repo contains my personal collection of VulnHub machine write-ups. Each folder includes my process and solutions to help others learn enumeration, exploitation, and privilege escalation.

---

## 📁 Machines Covered

| #  | Machine           | Difficulty     | Highlights                                                   | Flags      | Write-up                                                                                                                      |
| -- | ----------------- | -------------- | ------------------------------------------------------------ | ---------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 01 | Empire: LupinOne  | 🟡 Easy-Medium | SSH Key discovery, Base58, pip privesc                       | user, root | Empire-LupinOne                                                                                                               |
| 02 | Corrosion: 2      | 🟠 Medium      | NFS misconfig, CVE-2021-4034 (pwnkit), shadow cracking       | user, root | Corrosion-2                                                                                                                   |
| 03 | DC: 7             | 🟡 Easy-Medium | Drupalgeddon2, local file inclusion, MySQL password reuse    | user, root | DC-7                                                                                                                          |
| 04 | DarkHole: V2      | 🟠 Medium      | Misconfigured services, password reuse, sudo privilege abuse | user, root | DarkHole-V2                                                                                                                   |
| 05 | Hacker Kid: 1.0.1 | 🟢 Easy        | ptrace abuse, SUID binary, hardcoded creds, reverse shell    | user, root | [Hacker Kid-1.0.1](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Hacker-kid/Hacker%20Kid-1.0.1.md) |

---

## 🖋️ Empire: LupinOne (Easy‑Medium)

* **Ports**: 22 (SSH), 80 (HTTP)
* **Enum**: Hidden dir `/~secret/`, Base58 key
* **Access**: SSH with cracked private key
* **Privesc**: `pip` hijacking for root access
* **Flags**: `user.txt`, `root.txt`

---

## 🖋️ Corrosion: 2 (Medium)

* **Ports**: 21 (FTP), 2049 (NFS), 80 (HTTP)
* **Enum**: Anonymous NFS mount reveals SSH keys and shadow file
* **Access**: Cracked password from shadow dump via John the Ripper
* **Privesc**: Exploited `CVE-2021-4034` (Polkit - pwnkit)
* **Flags**: `user.txt`, `root.txt`

---

## 🖋️ DC: 7 (Easy‑Medium)

* **Ports**: 80 (HTTP), 3306 (MySQL)
* **Enum**: Drupal CMS, exploit Drupalgeddon2 (CVE-2018-7600)
* **Access**: Reused MySQL password for user shell
* **Privesc**: Abuse of LFI to read sensitive configs
* **Flags**: `user.txt`, `root.txt`

---

## 🖋️ DarkHole: V2 (Medium)

* **Ports**: 22 (SSH), 80 (HTTP)
* **Enum**: Discovered reused passwords via password spraying
* **Access**: Weak credentials on login
* **Privesc**: Exploited sudo permission misconfiguration
* **Flags**: `user.txt`, `root.txt`

---

## 🖋️ Hacker Kid: 1.0.1 (Medium)

* **Ports**: 52 (DNS), 80 (HTTP), 9999 (HTTP)
* **Enum**: Web-based enumeration, the name parameter is vulnerable to injection attack
* **Access**: Used reverse shell to gain user shell
* **Privesc**: Used `ptrace` and SUID binary for privilege escalation

---

## 📜 License

This repository is for educational and research purposes only. All content is created from publicly available vulnerable machines.
