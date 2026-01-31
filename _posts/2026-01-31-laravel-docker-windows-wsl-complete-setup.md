---
title: "Running Laravel with Docker on Windows the Right Way (WSL Setup Guide)"
date: 2026-01-31 10:00:00 +0530
layout: single
classes: wide
author_profile: false
read_time: true
toc: true
toc_sticky: true
description: "A battle-tested guide to correctly setting up WSL, Docker Desktop, and VS Code on Windows to run Laravel projects without permission issues or Docker volume errors."
categories: [docker, laravel, wsl, setup]
tags: [docker, laravel, wsl, windows, vscode]
---

# Windows → WSL → Docker → VS Code (WSL) → Laravel Guide
    
    **One-Time Machine Setup + Daily Workflow (BookStack Example)**
    
    This is the **correct machine setup** for running Dockerized Laravel projects on Windows.
    
    > ⚠️ **Warning:** If you do this in the wrong order or on the wrong drive, you will face:
    > * Permission issues
    > * Slow performance
    > * Docker volume mount problems
    > * Random Laravel `storage/cache` bugs
    
    ---
    
    ## 🏗️ PART 1: One-Time Machine Setup
    
    ### 🧱 STEP 1 — Install WSL + Ubuntu (FIRST)
    1. Open **PowerShell as Administrator**.
    2. Run the following command:
       ```powershell
       wsl --install -d Ubuntu

3.  **Restart your computer** when asked.
    
4.  After restart, the Ubuntu terminal will open automatically.
    
    -   Set your **username**.
        
    -   Set your **password**.
        

_You now have a real Linux environment inside Windows._

### 🧱 STEP 2 — Update WSL Kernel (Required for Docker)

Run this in PowerShell or Command Prompt:

PowerShell

    wsl --update

_This avoids Docker ↔ WSL compatibility issues._

### 🧱 STEP 3 — Install Docker Desktop (Windows)

1.  Download and install **Docker Desktop for Windows**.
    
2.  Open Docker Desktop.
    
3.  Wait until the bottom left status bar shows: **Docker is running**.
    

### 🧱 STEP 4 — Connect Docker with WSL (Critical Step)

1.  Open Docker Desktop.
    
2.  Go to **Settings (⚙️) → Resources → WSL Integration**.
    
3.  Check the box **Enable integration with my default WSL distro**.
    
4.  **Turn ON** the toggle next to `Ubuntu`.
    
5.  Click **Apply & Restart**.
    

_Now Docker runs inside Linux, not Windows._

### 🧱 STEP 5 — Verify Docker inside WSL

1.  Open your **Ubuntu terminal**.
    
2.  Run:
    
    Bash
    
        docker --version
    

_If a version number appears, integration is successful._

### 🧱 STEP 6 — Fix Docker Permission (Very Important)

Inside your **Ubuntu** terminal run:

Bash

    sudo usermod -aG docker $USER

1.  **Close** the Ubuntu window completely.
    
2.  **Open** Ubuntu again.
    
3.  Test by running:
    
    Bash
    
        docker ps
    

_No permission error should appear._

### 🧱 STEP 7 — The Golden Rule (Most Important)

> ❌ **NEVER** use Windows drives for Docker projects (`/mnt/c`, `/mnt/d`, etc.).
> 
> This is the root cause of slow performance and file permission bugs.

> ✅ **ALWAYS** work inside the Linux home directory:
> 
> `/home/your-username`

### 🧱 STEP 8 — Clone Project Inside Linux Home

Inside Ubuntu:

Bash

    cd ~
    git clone [https://github.com/BookStackApp/BookStack.git](https://github.com/BookStackApp/BookStack.git)
    cd BookStack

### 🧱 STEP 9 — Open in VS Code (WSL) → Then Run Docker

1.  From inside the project folder in Ubuntu:
    
    Bash
    
        code .
    
2.  VS Code will open. **Check the bottom-left corner**. It must show:
    
    > **\>< WSL: Ubuntu**
    
3.  Open the terminal inside VS Code (`` Ctrl + ` ``).
    
4.  Start Docker containers:
    
    Bash
    
        docker compose up -d
    
5.  Open your browser to: [http://localhost:8080](https://www.google.com/search?q=http://localhost:8080&authuser=1)
    

* * *

## 🧭 PART 2: Daily Workflow — Start / Stop Routine

This is the exact routine to start and stop your Laravel/PHP projects using WSL.

### 🟢 START WORK (from WSL)

1.  **Open Ubuntu (WSL)**
    
    -   Start Menu → Ubuntu OR PowerShell type `wsl`
        
2.  **Go to your Linux home**
    
    Bash
    
        cd ~
    
3.  **Go to project folder**
    
    Bash
    
        cd BookStack
    
4.  **Start Docker containers**
    
    Bash
    
        docker compose up -d
    
5.  **Open VS Code in WSL mode**
    
    Bash
    
        code .
    
    _Check bottom-left confirms: `WSL: Ubuntu`_
    
6.  **Open in browser**
    
    -   [http://localhost:8080](https://www.google.com/search?q=http://localhost:8080&authuser=1)
        

### 🔴 END WORK (Proper Shutdown)

1.  **Close VS Code remote connection**
    
    -   Press: `Ctrl + Shift + P`
        
    -   Type: `Close Remote Connection`
        
    -   Close the window.
        
2.  **Stop Docker containers (from WSL terminal)**
    
    Bash
    
        docker compose down
    
3.  **Exit Ubuntu**
    
    Bash
    
        exit