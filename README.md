# 🛡️ VulnHub Machines — Walkthrough Archive

Welcome to my personal collection of **VulnHub** machine walkthroughs. This repository documents my approach to solving vulnerable machines, focusing on **enumeration**, **exploitation**, and **privilege escalation** techniques — ideal for OSCP-style practice.

---

## 🔍 Why This Repo?

This archive is a learning resource to:
- Practice and refine penetration testing techniques
- Share write-ups with others preparing for exams or CTFs
- Document successful exploits and privilege escalation paths

---

## 📁 Covered Machines

| #  | Machine Name       | Difficulty     | Key Techniques Used                                             | Flags       | Write-up Link                                                                                                                |
|----|--------------------|----------------|------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------|
| 01 | Empire: LupinOne   | 🟡 Easy-Medium | SSH key discovery, Base58 decoding, `pip` privilege escalation   | user, root  | [Empire-LupinOne](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Empire-LupinOne/Empire%20LupinOne.md)                                                                                                              |
| 02 | Corrosion: 2       | 🟠 Medium      | NFS misconfiguration, shadow cracking, `pwnkit` (CVE-2021-4034) | user, root  | [Corrosion-2](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Corrosion2/Corrosion2.md)                                                                                                                  |
| 03 | DC: 7              | 🟡 Easy-Medium | Drupalgeddon2, MySQL password reuse, LFI                         | user, root  | [DC-7](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/DC-7/DC-7.md)                                                                                                                         |
| 04 | DarkHole: V2       | 🟢 Easy      | Reused credentials, sudo abuse                                  | user, root  | [DarkHole-V2](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/DarkHole-V2/DarkHole-V2.md)                                                                                                                  |
| 05 | Hacker Kid: 1.0.1  | 🟠 Medium        | SUID binary, hardcoded credentials, `ptrace` abuse              | user, root  | [Hacker Kid-1.0.1](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Hacker-kid/Hacker%20Kid-1.0.1.md) |
| 06 | DC: 9   | 🟠 Medium | SQL Injection, port knocking, LFI,    | user, root  | [Empire-LupinOne](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Empire-LupinOne/Empire%20LupinOne.md) 
| 07 | W34kn3ss: 1   | 🟠 Medium | Hidden directory, vulnerable openssl, compile python script     | user, root  | [W34kn3ss-1](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/W34kn3ss-1/W34KN3SS-1.md)
| 08 | Cereal:1   | 🟠 Medium |   PHP Object Injection, pspy help us find the script own by root, create symbolic-link for root access   | local, proof  | [cereal-1](https://github.com/PritamSuryawanshii/VulnHub-Machines/blob/main/Writeup/Cereal-1/Cereal-1.md)

---

## ⚔️ Key Techniques Used

This repo explores a wide range of real-world offensive security skills:

- 🔎 **Enumeration**
  - Hidden directories & credentials
  - NFS/FTP misconfigurations
  - Drupal vulnerabilities (e.g., Drupalgeddon2)

- 🛠️ **Initial Access**
  - Cracked private SSH keys
  - CMS exploitation
  - Reverse shell via injection
  - PHP Object Injection

- 🚀 **Privilege Escalation**
  - SUID binary exploitation
  - `pip` and `sudo` misconfigurations
  - Kernel/local exploits like `pwnkit`
  - Abuse of `ptrace` and hardcoded creds
  - create symbolic link for root access

---

## ⚖️ License & Disclaimer

This repository is intended for **educational and research purposes only**. All walkthroughs are based on publicly available machines hosted on [VulnHub](https://www.vulnhub.com). Please use responsibly.

---
