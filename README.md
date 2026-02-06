# 📥 .NET Remote Installer & Server Patching

**PowerShell automation for remote .NET Framework deployment and Windows Server patching — no external modules required.**
![Version](https://img.shields.io/badge/version-1.0-00d4ff)
![Status](https://img.shields.io/badge/status-Active-008000)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1+-a855f7?logo=powershell&logoColor=white&labelColor=16161f)

---

## 💡 What It Does

This repository contains two PowerShell scripts for enterprise server management. Both are designed to run from a central management server against multiple remote targets — no agents, no external modules, no internet required.

---

## 📋 Table of Contents

- [Script 1: .NET Framework 4.8 Installer](#-script-1-net-framework-48-installer)
- [Script 2: Windows Server Patching](#-script-2-windows-server-patching)
- [Prerequisites](#-prerequisites)
- [License](#-license)

---

## ⚡ Script 1: .NET Framework 4.8 Installer

`install-dotnet48.ps1` — Automates .NET Framework 4.8 installation on remote Windows Servers using an existing offline installer.

### Features

- **Pre-flight check** — Skips installation if .NET Framework 4.8 (Release Key ≥ 528040) is already present
- **Admin validation** — Verifies the PowerShell session runs with administrator privileges
- **Quiet mode** — Runs the installer with `/q /norestart` flags
- **Post-install verification** — Validates the installed .NET version after completion
- **Smart exit codes** — Treats both `0` (success) and `3010` (reboot required) as successful, logs the reboot requirement but does **not** auto-restart

### Usage

```powershell
# Basic usage — point to your offline installer on a network share
.\install-dotnet48.ps1 -InstallerPath "\\fileserver\share\ndp48-x86-x64-allos-enu.exe"

# Custom log path
.\install-dotnet48.ps1 -InstallerPath "\\fileserver\share\ndp48-x86-x64-allos-enu.exe" `
                       -LogPath "D:\Logs\dotnet48.log"
```

### Parameters

| Parameter | Required | Default | Description |
|:----------|:--------:|:-------:|:------------|
| `-InstallerPath` | ✅ | — | Path to the .NET 4.8 offline installer EXE (e.g. UNC path) |
| `-LogPath` | ❌ | `%TEMP%\dotnet48-install.log` | Custom log file location |

---

## ⚡ Script 2: Windows Server Patching

`Patch-WindowsServers.ps1` — Remotely patches multiple Windows Servers from a central system using the built-in Windows Update Agent API.

### Features

- **No external modules** — Uses the Windows Update Agent COM API via a temporary scheduled task running as SYSTEM. No `PSWindowsUpdate`, no third-party dependencies
- **Multi-server support** — Patches a list of servers in sequence from one management machine
- **Before/after comparison** — Reports OS version and build number before and after patching
- **Smart reboot handling** — Reboots servers when required and waits for them to come back online
- **Live terminal output** — Real-time progress reporting during execution
- **CSV export** — Generates a report file in the current directory after completion

### Usage

```powershell
# Define target servers
$servers = "APP01", "DB01", "WEB03"

# Run with admin privileges
.\Patch-WindowsServers.ps1 -Servers $servers
```

### How It Works

1. Connects to each server via PowerShell Remoting (WinRM)
2. Creates a temporary scheduled task under `C:\ProgramData\Remote-Patching`
3. The task runs as SYSTEM and calls the Windows Update Agent API to search and install updates
4. Equivalent to clicking "Check for updates" → "Install now" in Windows Settings
5. Reports results and cleans up the temporary task

### Supported Targets

- Windows Server 2016
- Windows Server 2019
- Windows Server 2022

---

## 📋 Prerequisites

- PowerShell 5.1 or newer on the management server
- PowerShell Remoting (WinRM) enabled on all target servers
- Domain or local admin credentials for the target servers
- No internet access required — updates can be pre-staged via WSUS or manual deployment
- Task Scheduler must be available on target servers

---

## 🛠️ Tech Stack

![PowerShell](https://img.shields.io/badge/PowerShell-16161f?style=flat-square&logo=powershell&logoColor=00d4ff)
![Windows Server](https://img.shields.io/badge/Windows_Server-16161f?style=flat-square&logo=windows&logoColor=00d4ff)

```
.net-Remote-Installer/
├── install-dotnet48.ps1         # .NET Framework 4.8 installer
├── Patch-WindowsServers.ps1     # Remote patching automation
└── README.md
```

---

## 📄 License

Unlicensed — provided as-is for enterprise use.

---

<p align="center">
  <a href="https://cheswick.dev">
    <img src="https://img.shields.io/badge/CHESWICK.DEV-00d4ff?style=for-the-badge&logo=firefox&logoColor=0a0a0f&labelColor=a855f7" alt="cheswick.dev" />
  </a>
</p>

<p align="center">
  Made with 🖤 by <a href="https://cheswick.dev">cheswick.dev</a>
</p>
