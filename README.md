# 🦉 OCryptSim

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux-0a0c10?style=flat-square"/>
  <img src="https://img.shields.io/badge/Category-OAttack%20/%20Offensive-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OCryptSim** — Ransomware Behavior Simulator ║

---

> ⚠️ **AUTHORISED USE ONLY** — Use only on systems you own or have explicit written permission to test. Unauthorised use is illegal.

---

## 📌 Overview

OCryptSim safely emulates ransomware-like behaviour chains inside an approved /tmp sandbox for purple-team and detection engineering. It is non-destructive by default (dry-run/monitor) and only writes reversible artefacts when explicitly allowed. Use it to exercise EDR, XDR, and SIEM detections without harming production data.

---

## ✨ Features

- Sandboxed ransomware-behaviour modules limited to an approved /tmp target
- Dry-run/monitor mode by default; --execute required to write reversible artefacts
- Selectable modules, speed controls, and capped file counts for safe drills
- Auto-cleanup and report/log paths constrained under the sandbox
- MITRE coverage export and detection-oriented reporting
- Config save/load for repeatable incident-response exercises

---

## ⚙️ Requirements

- **Linux**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage

```bash
./OCryptSim --dry-run --module 1 --count 10
./OCryptSim --target /tmp/ocryptsim_lab --execute --auto-cleanup
./OCryptSim --mitre-export --report-dir /tmp/ocryptsim_lab/reports
./OCryptSim --help
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
