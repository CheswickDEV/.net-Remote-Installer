# 📥 .NET Remote Installer & Server Patching

### PowerShell automation for remote .NET Framework deployment and Windows Server patching — no external modules required.

[![GitHub Stars](https://img.shields.io/github/stars/CheswickDEV/.net-Remote-Installer?color=00d4ff&labelColor=16161f)](https://github.com/CheswickDEV/.net-Remote-Installer)
[![Last Commit](https://img.shields.io/github/last-commit/CheswickDEV/.net-Remote-Installer?color=a855f7&labelColor=16161f)](https://github.com/CheswickDEV/.net-Remote-Installer/commits/main)
![Version](https://img.shields.io/badge/version-1.0-00d4ff?labelColor=16161f)
![Status](https://img.shields.io/badge/status-Active-00d4ff?labelColor=16161f)
![License](https://img.shields.io/badge/license-Unlicensed-6c757d?labelColor=16161f)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1+-a855f7?logo=powershell&logoColor=white&labelColor=16161f)

---

## 💡 What It Does

This repository contains two PowerShell scripts for enterprise server management. Both are designed to run from a central management server against multiple remote targets — no agents, no external modules, no internet required.

---

## 📋 Table of Contents

- [Script 1: .NET Framework 4.8 Installer](#-script-1-net-framework-48-installer)
- [Script 2: Windows Server Patching](#-script-2-windows-server-patching)
- [Prerequisites](#-prerequisites)
- [Tech Stack](#️-tech-stack)
- [Changelog](#-changelog)
- [License](#-license)

---

## ⚡ Script 1: .NET Framework 4.8 Installer

`install-dotnet48.ps1` — Automates .NET Framework 4.8 installation on remote Windows Servers using an existing offline installer.

### Features

- **✈️ Pre-flight Check** — Skips installation if .NET Framework 4.8 (Release Key ≥ 528040) is already present.

- **🔐 Admin Validation** — Verifies the PowerShell session runs with administrator privileges.

- **🤫 Quiet Mode** — Runs the installer with `/q /norestart` flags.

- **✅ Post-install Verification** — Validates the installed .NET version after completion.

- **📊 Smart Exit Codes** — Treats both `0` (success) and `3010` (reboot required) as successful, logs the reboot requirement but does **not** auto-restart.

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

- **🔌 No External Modules** — Uses the Windows Update Agent COM API via a temporary scheduled task running as SYSTEM. No `PSWindowsUpdate`, no third-party dependencies.

- **🖥️ Multi-server Support** — Patches a list of servers in sequence from one management machine.

- **📋 Before/After Comparison** — Reports OS version and build number before and after patching.

- **🔄 Smart Reboot Handling** — Reboots servers when required and waits for them to come back online.

- **📺 Live Terminal Output** — Real-time progress reporting during execution.

- **📄 CSV Export** — Generates a report file in the current directory after completion.

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

![PowerShell](https://img.shields.io/badge/PowerShell-16161f?logo=powershell&logoColor=00d4ff)
![Windows Server](https://img.shields.io/badge/Windows_Server-16161f?logo=windows&logoColor=00d4ff)

```
.net-Remote-Installer/
├── install-dotnet48.ps1         # .NET Framework 4.8 installer
├── Patch-WindowsServers.ps1     # Remote patching automation
└── README.md
```

---

## 📝 Changelog

### v1.0 (current)
- 🚀 Initial release
- ✨ .NET Framework 4.8 remote installer with pre-flight checks
- ✨ Windows Server patching via Windows Update Agent API
- ✨ CSV report export

---

## 📄 License

Unlicensed — provided as-is for enterprise use.

---

<p align="center">
  <a href="https://cheswick.dev">
    <img src="https://img.shields.io/badge/CHESWICK.DEV-00d4ff?logo=firefox&logoColor=0a0a0f&labelColor=a855f7" alt="cheswick.dev" />
  </a>
</p>

<p align="center">
  Made with 🖤 by <a href="https://cheswick.dev">cheswick.dev</a>
</p>
