# 🦉 OCryptSim

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-OAttack%20%2F%20Red%20Team-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-2.0-cyan?style=flat-square"/>
</p>

> **OCryptSim** is a safe, non-destructive ransomware behaviour simulator for EDR / XDR / SIEM detection testing and incident response drills.
> No real encryption ever occurs. All destructive commands are logged only — never executed.

---

> ⚠️ **AUTHORISED RED TEAM / SECURITY TESTING USE ONLY**
>
> - ✅ No real encryption occurs — all `.locked` files are harmless plain-text
> - ✅ No real commands are executed — shadow/registry/persistence steps are **logged only**
> - ✅ No real network connections — C2 and DNS activity is **simulated locally**
> - ✅ All artifacts are contained inside `OCRYPTSIM_TEST/` subdirectory

---

## 📌 Overview

OCryptSim emulates the full behavioural chain of a ransomware attack — from initial file discovery through encryption, ransom note delivery, C2 communication, persistence, and backup destruction — generating realistic telemetry that your security stack should detect, without any actual damage.

---

## 🖥️ Modules

| # | Module | Description |
|---|--------|-------------|
| **1** | **File Discovery** | Walk the target directory, `stat` every file, read 4 KB chunks, and compute SHA-256 hashes — mimicking ransomware recon |
| **2** | **Encryption Sim** | Create configurable number of fake `.locked` / `.enc` / `.crypt` files inside `OCRYPTSIM_TEST/` — plain-text with simulated key-ID and AES-IV markers |
| **3** | **Ransom Note Drop** | Drop 5 ransom note variants (TXT + HTML) into the target and sim directories — `README_RECOVER.txt`, `HOW_TO_DECRYPT.html`, and more |
| **4** | **C2 / Network Sim** | Simulate DNS resolution of C2 domains, outbound TCP `:443` connections, data exfiltration, and Tor hidden service lookups — logged locally, no real traffic |
| **5** | **Persistence Sim** | Log registry run key writes, mutex creation, scheduled task, service install, and WMI event subscription — all written to `persistence_sim.txt`, never executed |
| **6** | **Shadow Delete Sim** | Log `vssadmin`, `bcdedit`, `wmic`, `wbadmin`, and `diskshadow` shadow copy deletion commands — written to `shadow_delete_sim.txt`, never executed |
| **7** | **Full Chain** | Run all 6 modules in sequence with a summary of discovered files, encrypted files, notes dropped, C2 domains hit, and artifacts created |
| **8** | **Cleanup** | Remove all simulation artifacts — deletes `OCRYPTSIM_TEST/` directory and all ransom note files |

---

## 🛡️ Expected Detections

Use the built-in **[D] Detections** reference screen to see exactly what your EDR/XDR/SIEM should alert on:

**File System**
- Mass file access — single process reading hundreds of files rapidly
- Rapid file renaming — `*.docx.locked` · `*.pdf.enc` · `*.jpg.crypt` pattern
- Suspicious file creation — `README_RECOVER.txt` / `HOW_TO_DECRYPT.html` in directories
- Test directory creation — `OCRYPTSIM_TEST/` folder with many `.locked` files

**Network**
- Suspicious DNS queries — `ransom-c2[.]onion` · `decryptor[.]xyz` · `btc-recover[.]net`
- Unusual outbound TCP `:443` following a file-access spike
- Data exfiltration — large outbound transfer after file enumeration
- Tor / SOCKS proxy — `.onion` DNS resolution or SOCKS5 connection attempt

**Process & Registry**
- Registry run key write — `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
- Named mutex — `Global\OCryptSim_Mutex_2026`
- Scheduled task creation — `schtasks /create /ru SYSTEM /sc onlogon`
- Service install — `sc create OCryptSimSvc`
- WMI event subscription — `CommandLineEventConsumer / OCryptTrigger`

**Backup Destruction**
- `vssadmin delete shadows /all /quiet`
- `bcdedit /set {default} recoveryenabled No`
- `wmic shadowcopy delete`
- `wbadmin delete catalog -quiet`
- `diskshadow /s shadow_delete.txt`

---

## ⚙️ Settings

Configurable from the **[S] Settings** menu:

| Setting | Default | Description |
|---------|---------|-------------|
| Target directory | `~/` | Root directory for file discovery and artifact placement |
| File count | `20` | Number of fake encrypted files to create |
| Discovery limit | `500` | Max files to enumerate during File Discovery |
| File delay | `0.04s` | Delay between file operations |
| Auto-cleanup | Off | Automatically remove artifacts after Full Chain |
| Save report | On | Save JSON report to `ocryptsim_results/` after each module |

---

## 📤 Reports

Each module run saves a JSON report to:

```
ocryptsim_results/ocryptsim_YYYYMMDD_HHMMSS.json
```

Reports include tool metadata, target path, modules run, stats per module, and a full list of created artifacts.

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage

```bash
./OCryptSim
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
