---
layout: default
title: Working with Remote Servers & HPC
parent: CS100 Computer Fundamentals
nav_order: 3
---

# Working with Remote Servers & High-Performance Computing (HPC)

Because next-generation sequencing datasets often require terabytes of storage and hundreds of gigabytes of RAM, virtually all serious computational biology takes place on **remote servers**, **cloud compute nodes**, or **institutional HPC clusters**.

---

## 1. Why Remote Computing?

A typical high-throughput sequencing run produces tens to hundreds of gigabytes of raw data per sample. Your personal laptop:
1. Cannot stay powered on and connected to the internet for days at a time without interruption.
2. Does not have the 64–512 GB of RAM required for human genome assembly or single-cell RNA-seq clustering.
3. Cannot parallelize across 64+ CPU cores.

---

## 2. Remote Access: Secure Shell (SSH)

### How SSH Works
SSH (Secure Shell) creates an encrypted tunnel between your local computer (the client) and the remote machine (the server).
* **Password vs. SSH Key Authentication**: 
  * Passwords can be brute-forced and require typing on every login.
  * **SSH Key Pairs** (Public Key on server, Private Key on client) provide cryptographic authentication. This enables secure, automated remote scripts and file synchronization (`rsync`, `scp`).

### Remote IDEs (VS Code Remote-SSH)
Instead of editing code on your laptop and manually uploading files to the server, modern tools like VS Code's **Remote - SSH** extension run a headless language server directly on the remote machine. You get the local desktop GUI experience with all compute and file access executing remotely.

---

## 3. Persistent Sessions: Why You Need Terminal Multiplexers (`tmux`)

When you run a command directly in an SSH terminal session, the process is a child of that specific SSH connection.
* **The Problem**: If your Wi-Fi drops, your laptop closes, or the connection times out, the SSH session dies and sends a `SIGHUP` (Hangup Signal) to all running processes, killing your 12-hour alignment job midway.
* **The Rationale for `tmux`**:
  * `tmux` (terminal multiplexer) runs a server daemon *on the remote machine*.
  * Inside `tmux`, your terminal sessions stay alive independently of your network connection.
  * You can disconnect, go home, reconnect to SSH, attach back to your `tmux` session, and find your job still running exactly where you left it.

---

## 4. Environment Configuration & The `$PATH`

When you type a command like `python` or `samtools`, the operating system needs to know which executable file to run.
* The system checks the **`$PATH`** environment variable—an ordered list of directories separated by colons (`:`).
* Configuration files (like `~/.bashrc` or `~/.zshrc`) are shell scripts that execute automatically every time you log in, configuring your `$PATH`, tool aliases, and environment variables.

---

## 5. Shared Clusters & Job Schedulers (SLURM / PBS)

On a personal server, you can execute commands directly on the terminal (the "login node"). On a shared institutional cluster with hundreds of researchers, running heavy computation on the login node slows down the server for everyone.

### The Role of Schedulers (e.g., SLURM)
1. **Queueing & Fair Sharing**: Users submit jobs specifying the required resources (e.g., `#SBATCH --cpus-per-task=16 --mem=64G --time=08:00:00`).
2. **Resource Allocation**: The scheduler finds available compute nodes that match the requested CPU/RAM resources, launches the job in isolation, and sends email alerts when finished.
3. **Reproducibility**: Writing SLURM batch scripts forces researchers to declare exact resource requirements and dependencies explicitly.

> **Takeaway**: Use SSH keys for secure remote access, `tmux` to prevent interrupted runs, and batch submission scripts (e.g. SLURM) to request shared cluster resources safely.
