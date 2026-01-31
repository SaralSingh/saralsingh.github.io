---
title: "Running Laravel with Docker on Windows the Right Way (WSL Setup Guide)"
date: 2026-01-31
description: "A battle-tested guide to correctly setting up WSL, Docker Desktop, and VS Code on Windows to run Laravel projects without permission issues or Docker volume errors."
categories: [docker, laravel, wsl, setup]
tags: [docker, laravel, wsl, windows, vscode]
---

# 🚀 Windows → WSL → Docker → VS Code (WSL) → Laravel
## One-Time Machine Setup + Daily Workflow (BookStack Example)

This is the **correct machine setup** for running Dockerized Laravel projects on Windows.

If you do this in the wrong order, you will face:

- Permission issues
- Slow performance
- Docker volume mount problems
- Random Laravel `storage/cache` bugs

Follow **exactly**.

---

## 🧱 STEP 1 — Install WSL + Ubuntu (FIRST)

Open **PowerShell as Administrator**:

```powershell
wsl --install -d Ubuntu
```

Restart when asked.

After restart:

- Ubuntu terminal opens
- Set your username
- Set your password

You now have a real Linux environment inside Windows.

---

## 🧱 STEP 2 — Update WSL Kernel (Required for Docker)

```powershell
wsl --update
```

This avoids Docker ↔ WSL compatibility issues.

---

## 🧱 STEP 3 — Install Docker Desktop (Windows)

- Download and install Docker Desktop
- Open Docker Desktop
- Wait until it shows: **Docker is running**

---

## 🧱 STEP 4 — Connect Docker with WSL (Critical Step)

Open:

**Docker Desktop → Settings → Resources → WSL Integration**

- Enable WSL Integration
- Turn ON **Ubuntu**
- Click **Apply & Restart**

Now Docker runs inside Linux, not Windows.

---

