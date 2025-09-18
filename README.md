---

# 🔍 DLL Sideloading Risk Detector

This tool automates the detection of **DLL sideloading opportunities** on Windows systems using [Procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon).
It captures DLL load behavior, analyzes search paths, and highlights **possible attack surfaces** where an attacker could hijack the DLL loading process.

---

## ✨ Features

* **Automated Procmon Capture** – runs Procmon in headless mode, captures DLL load activity, and exports logs.
* **DLL Load Analysis** – identifies suspicious DLL search patterns (e.g., missing DLLs or relative paths).
* **Noise Filtering** – ignores irrelevant system processes (e.g., `procmon.exe`, `backgroundtaskhost.exe`).
* **Heuristic Checks** – instead of requiring admin vs. standard user checks, the tool now:

  * Detects EXEs using **relative paths** for DLLs.
  * Flags DLL lookups that fall into **user-writable locations** (e.g., `%TEMP%`, `C:\Users\Public\`).
  * Adds an optional check to see whether the application verifies **DLL digital signatures**.

---

## 📋 Example Output

```
[Possible DLL Side Loading] agentInstallerComponent.exe tried to load textshaping.dll
  ❌ Lookup failed: c:\windows\temp\textshaping.dll
    ⚠️ Writable location (TEMP) — attacker controlled
    🚫 No signature validation detected
```

---

## 🛠️ Requirements

* Windows
* [Procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) in PATH
* Python 3.8+

---

## ▶️ Usage

1. Run the script:

   ```bash
   python sideload_check.py
   ```

2. The script will:

   * Start Procmon (headless mode)
   * Capture DLL load activity
   * Export logs to CSV
   * Analyze for **possible DLL sideloading risks**
   * Classify results with heuristic checks

3. To run silently (no console window):

   ```bash
   pythonw sideload_check.py
   ```

---

## ⚡ How It Works

* Many applications probe multiple directories before successfully loading a DLL.
* If any of these directories are **user-writable**, an attacker may drop a malicious DLL and hijack the process.
* The tool flags **possible risks** using three checks:

  1. EXE loads DLLs via relative paths
  2. DLL lookup paths overlap with known **user-writable locations**
  3. DLL signature validation is missing or not enforced

---

## 📌 Notes

* This tool is for **detection and auditing only** — it does not block DLL sideloading.
* Designed for **blue teams, DFIR analysts, and EDR developers**.
* Results can be extended to **export JSON/CSV** for SIEM or pipeline integration.

---

### 🔖 GitHub Repo Description (short tagline)

> Detects **possible DLL sideloading risks** on Windows by analyzing Procmon logs and applying heuristic checks (relative DLL paths, writable directories, missing signature validation).

---
