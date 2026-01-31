---
title: "Running Laravel with Docker on Windows the Right Way (WSL Setup Guide)"
date: 2026-01-31
layout: single
classes: narrow
description: "A battle-tested guide to correctly setting up WSL, Docker Desktop, and VS Code on Windows to run Laravel projects without permission issues or Docker volume errors."
categories: [docker, laravel, wsl, setup]
tags: [docker, laravel, wsl, windows, vscode]
---


# Windows → WSL → Docker → VS Code (WSL) → Laravel
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

## 🧱 STEP 5 — Verify Docker inside WSL

Open Ubuntu terminal:

```bash
docker --version
```

If version appears → integration successful.

---

## 🧱 STEP 6 — Fix Docker Permission (Very Important)

Inside Ubuntu:

```bash
sudo usermod -aG docker $USER
```

Close Ubuntu. Open it again.

Test:

```bash
docker ps
```

No permission error should appear.

---

## 🧱 STEP 7 — Golden Rule (Most Important)

❌ Never use Windows drives for Docker projects:

```
/mnt/c
/mnt/d
/mnt/e
```

This is the root cause of most Docker + Laravel problems.

✅ Always work inside Linux home:

```
/home/your-username
```

---

## 🧱 STEP 8 — Clone Project Inside Linux Home (Example: BookStack)

```bash
cd ~
git clone https://github.com/BookStackApp/BookStack.git
cd BookStack
```

---

## 🧱 STEP 9 — Open in VS Code (WSL) → Then Run Docker

From Ubuntu (inside project folder):

```bash
code .
```

Check the bottom-left of VS Code. It must show:

`WSL: Ubuntu`

Open the terminal inside VS Code using the default terminal shortcut.


Start Docker containers from this terminal:

```bash
docker compose up -d
```

Open in browser:

```
http://localhost:8080
```

Project should now be running.

---

# 🧭 Daily Workflow — Start / Stop Routine

This is the exact routine to start and stop your Laravel project (BookStack or any Docker-based app) using **WSL (Ubuntu)** and **VS Code**.

---

## 🟢 START WORK (from WSL)

### 1) Open Ubuntu (WSL)
- Start Menu → **Ubuntu**
- OR PowerShell:

```bash
wsl
```

---

### 2) Go to your Linux home

```bash
cd ~
```

---

### 3) Go to project folder

```bash
cd BookStack
```

---

### 4) Start Docker containers

```bash
docker compose up -d
```

---

### 5) Open VS Code in WSL mode

```bash
code .
```

Check bottom-left in VS Code shows:

```
WSL: Ubuntu
```

---

### 6) Open in browser

```
http://localhost:8080
```

Now start coding.

---

## 🔴 END WORK (proper shutdown)

### 1) Close VS Code remote connection

In VS Code:

- Press: `Ctrl + Shift + P`
- Type: `Close Remote Connection`
- Close the VS Code window

---

### 2) Stop Docker containers (from WSL)

```bash
docker compose down
```

---

### 3) Exit Ubuntu

```bash
exit
```

---

