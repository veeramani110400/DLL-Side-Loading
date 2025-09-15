
---

# 🔍 DLL Sideloading Risk Detector

This tool automates the detection of **DLL sideloading opportunities** on Windows systems using [Procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon).
It captures DLL load behavior, analyzes search paths, and checks whether those paths are writable by a **standard user** — highlighting real-world attack surfaces.

---

## ✨ Features

* **Automated Procmon Capture** – runs Procmon in headless mode, captures events, and exports logs.
* **DLL Load Analysis** – identifies suspicious DLL search patterns (`CreateFile → not found` + `Load Image → success`).
* **Writable Path Detection** – verifies if DLL search paths are writable **as a standard user**, not as Administrator.
* **Noise Filtering** – ignores irrelevant system processes (e.g., `procmon.exe`, `backgroundtaskhost.exe`).
* **Focused Output** – only shows **non-protected, writable paths** where an attacker could actually drop or replace a DLL.

---

## 📋 Example Output

```
[Writable DLL sideloading risk] agentInstallerComponent.exe tried to load textshaping.dll
  ❌ Failed CreateFile: c:\windows\temp\textshaping.dll
    🔓 Writable (standard user) → attacker can drop DLL here
```

---

## 🛠️ Requirements

* Windows (Admin rights required for Procmon capture)
* [Procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) in PATH
* Python 3.8+

---

## ▶️ Usage

1. Run the main script as **Administrator**:

   ```bash
   python sideload_check.py
   ```

2. The script will:

   * Start Procmon (headless)
   * Capture DLL activity
   * Export to CSV
   * Analyze for sideloading risks
   * Test path writability as a **standard user** via a helper script

3. To run headless (no console window):

   ```bash
   pythonw sideload_check.py
   ```

---

## ⚡ How It Works

* Many applications probe multiple directories before successfully loading a DLL.
* If any of these directories are **writable by an unprivileged user**, an attacker can drop a malicious DLL to hijack the trusted process.
* This tool flags only **actual risks** (paths writable by a standard user).

---

## 📌 Notes

* The tool is for **detection and auditing** only — it does not block DLL sideloading.
* Designed for **blue teams, DFIR analysts, and EDR developers**.
* Results can be extended to **export JSON/CSV** for integration with SIEM or security pipelines.

---

### 🔖 GitHub Repo Description (short tagline)

> Detects **DLL sideloading risks** on Windows by analyzing Procmon logs and verifying writable DLL search paths as a standard user.

---

