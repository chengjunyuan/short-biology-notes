---
layout: default
title: Working with Remote Servers & HPC
parent: CS100 Computer Fundamentals
nav_order: 3
---

# Working with Remote Servers & High-Performance Computing (HPC)

While personal computers are ideal for writing code and prototyping small analyses, biological datasets (such as whole-genome sequencing libraries or single-cell atlases) routinely exceed the memory, storage, and processing limits of a laptop. 

To analyze large-scale data, computational work shifts to **remote servers**, **cloud compute instances**, and **High-Performance Computing (HPC) clusters**. Understanding remote architectures, connection protocols, shell configuration, and job scheduling is essential for running reliable, long-duration workflows.

---

## 1. What Actually is a Server? (Physical Reality, Networking, and Data Flow)

A **server** is not a mystical entity; it is physically a computer designed for continuous, high-throughput computation rather than interactive desktop use.

```
+-----------------------------------------------------------------------+
| Climate-Controlled Server Rack (Data Center)                         |
|                                                                       |
|  [ Rack Unit 1: Compute Node (128 Cores, 512 GB RAM, Dual 10GbE Fiber)]|
|  [ Rack Unit 2: GPU Node     (8x NVIDIA H100 GPUs, 1 TB RAM)          ]|
|  [ Rack Unit 3: Storage Array(10 Petabytes NVMe/HDD Lustre File System)]|
+-----------------------------------------------------------------------+
                                  ^
                                  | 10 - 100 Gbps High-Speed Wired Fiber
                                  v
                    [ Institutional Network Gateway ]
                                  ^
                                  | Encrypted Internet Tunnel (SSH)
                                  v
                   [ Your Local Laptop / Workstation ]
```

### Physical Architecture
* **Headless Design**: Servers have no monitors, keyboards, mice, or sound cards. They are thin, metal blade units (1U or 2U form factors) mounted inside standard 19-inch racks within climate-controlled, backup-powered data centers.
* **What is a "Node"?**: In cluster and network architecture, a **node** is simply an individual, standalone computer (or distinct virtual machine) connected as part of a unified network. A server rack consists of dozens of interconnected nodes.
* **Why No Wi-Fi?**: Servers almost never use Wi-Fi. Wi-Fi introduces packet loss, latency spikes, and interference. Servers connect directly to data center backbone switches using redundant, high-bandwidth wired **fiber-optic or Ethernet cables** (10 Gbps to 100+ Gbps).

### How Data Moves To and From a Server
Because there is no graphical desktop or USB flash drive plug-in workflow, data and software are transferred across the network:
* **Interactive Transfers**: Command-line utilities like **`rsync`** (for efficient, resumable directory synchronization) and **`scp`** (Secure Copy) push files from your laptop to the server.
* **Direct Server Downloads**: Using **`wget`** or **`curl`** on the server to download reference genomes and public databases directly from NCBI, Ensembl, or SRA at multi-gigabit data center network speeds.
* **High-Throughput Data Portals**: Tools like **Globus** or cloud storage CLIs (AWS S3, Google Cloud Storage) for transferring tens of terabytes between institutions.

---

## 2. Laptop vs. Remote Server: When to Use Which?

Choosing where to execute code is an engineering decision based on dataset scale, hardware limits, and turnaround time:

| Task / Characteristic | Local Laptop | Remote Server / HPC Cluster |
| :--- | :--- | :--- |
| **Writing & Editing Code** | Primary choice (local IDE, instant feedback). | Headless editing via SSH / VS Code Remote. |
| **Prototyping & Testing** | Ideal for testing scripts on 1,000-read subsets. | Overkill for simple debugging. |
| **Data Visualization & Plotting** | Fast rendering of summary figures and plots. | Generates PDF/PNG files to download or view via remote GUI. |
| **Memory Footprint** | Limited to 8–32 GB RAM. | Scales from 64 GB to multiple Terabytes of RAM. |
| **Processor Scale** | 4–16 CPU cores (laptop heats up and throttles). | 32–128+ physical cores on dedicated power. |
| **Execution Duration** | Hours (laptop must stay open, awake, and plugged in). | Days or weeks (runs unattended 24/7 in background). |
| **Specialized Hardware** | Standard integrated graphics. | Multi-GPU nodes (e.g., A100/H100) for AlphaFold and deep learning. |

---

## 3. Remote Access: Secure Shell (SSH)

When you type `ssh username@server.edu` in your terminal, how does your local machine talk to the remote server?

```
+-------------------+         Encrypted Network Tunnel (Port 22)        +--------------------+
| Local Laptop      | ------------------------------------------------> | Remote Server      |
| - SSH Client      |   Keystrokes encrypted & sent over internet       | - SSH Daemon (sshd)|
| - Terminal Window | <------------------------------------------------ | - Shell (Bash/Zsh) |
+-------------------+      stdout / stderr encrypted & streamed back    +--------------------+
```

### What SSH Does Under the Hood
1. **Encrypted Tunnel**: The **SSH (Secure Shell)** protocol establishes an encrypted, authenticated connection over TCP port 22 between your local **SSH Client** and the remote machine's background service (**`sshd`** - SSH daemon).
   * **What is a "Daemon"?**: In Unix/Linux systems, a **daemon** is a background process that runs continuously and quietly without an interactive window or attached terminal. It sleeps until a specific event or request occurs (e.g., `sshd` wakes up when a network connection arrives; `cron` wakes up at scheduled clock intervals). In Unix conventions, daemons typically have names ending with the letter `d` (such as `sshd`, `httpd`, `dockerd`, or `systemd`).
2. **Remote Execution**: When you type a command (e.g., `ls -lh`), your keystrokes are encrypted, sent over the network, decrypted by `sshd`, and passed to the shell running on the remote CPU.
3. **Stream Redirection**: The remote shell captures the program output (`stdout` and `stderr`), encrypts it, and streams it back across the internet to be drawn in your local terminal window.

### Authentication: Passwords vs. SSH Keys
* **Passwords**: Prone to brute-force attacks and require repetitive manual typing.
* **SSH Key Pairs (Cryptographic Authentication)**:
  * You generate a cryptographic key pair on your laptop (`ssh-keygen -t ed25519`): a **Private Key** (kept secure on your laptop, e.g., `~/.ssh/id_ed25519`) and a **Public Key** (`~/.ssh/id_ed25519.pub`).
  * You copy the public key to the server's `~/.ssh/authorized_keys` file (`ssh-copy-id username@server.edu`).
  * When connecting, the server uses public-key cryptography to challenge your client. Your laptop signs the challenge with the private key, authenticating you instantly without passwords.

### Modern Remote IDEs (VS Code Remote - SSH)
Instead of editing files in basic terminal text editors (`nano`, `vim`), modern tools like **VS Code Remote - SSH** connect over SSH and install a lightweight headless server process on the remote machine. You get the full graphical desktop code editor on your laptop while all file indexing, language tools, and terminal executions run on the server.

---

## 4. Persistent Sessions: Why You Need `tmux` (Terminal Multiplexers)

A critical limitation of raw SSH sessions is their dependency on an uninterrupted network connection.

### The Problem: Connection Drops and the `SIGHUP` Signal
Every command you run directly in an SSH terminal is a child process of that specific SSH connection:
* If your Wi-Fi disconnects, your laptop goes to sleep, or your VPN drops, the SSH connection breaks.
* The remote operating system detects the severed connection and sends a **`SIGHUP` (Signal Hangup)** signal to all processes attached to that terminal session.
* The OS immediately terminates your 14-hour sequence alignment or model training job halfway through.

```
SSH Disconnect Event:
Laptop Wi-Fi drops ---> SSH Connection Dies ---> Kernel sends SIGHUP ---> Alignment Process Killed!
```

### The Solution: `tmux` (Terminal Multiplexer)
**`tmux`** runs a persistent server daemon *on the remote machine itself*, detaching your programs from the fragile SSH connection.

```
+-----------------------------------------------------------------------+
| Remote Server                                                         |
|                                                                       |
|  [tmux Background Daemon]                                             |
|     |                                                                 |
|     +---> [Session: 'alignment'] -> Running bwa mem (Hours uninterrupted)|
|                                                                       |
+-----------------------------------------------------------------------+
       ^                                                 ^
       | Connected via SSH                               | Reconnected next day
+--------------+                                  +--------------+
| Laptop Day 1 |                                  | Laptop Day 2 |
+--------------+                                  +--------------+
```

### Essential `tmux` Workflow
1. **Start a named session**:
   ```bash
   tmux new -s analysis
   ```
2. **Launch your long-running job inside `tmux`** (e.g., `python train_model.py`).
3. **Detach from the session safely**:
   * Press `Ctrl + B`, release both keys, then press `D` (Detach).
   * You are returned to the regular server prompt. You can now close your laptop, disconnect Wi-Fi, or shut down your computer. The job continues running on the server.
4. **Reattach to the session later**:
   * Log back into the server via SSH from any computer and run:
     ```bash
     tmux attach -t analysis
     ```
   * Your terminal restores exactly where you left it, showing the active progress or completed output.

---

## 5. Shell Environment Configuration: `$PATH`, `.bashrc`, and `.zshrc`

When you type a command like `python`, `samtools`, or `git`, how does the operating system find the executable?

### The `$PATH` Environment Variable
The operating system does not scan your entire hard drive for commands. Instead, it checks the **`$PATH`** environment variable—an ordered, colon-separated list of directory paths:

```bash
$ echo $PATH
/home/user/.local/bin:/usr/local/bin:/usr/bin:/bin
```

When you type `samtools`, the shell checks each folder from left to right:
1. Does `/home/user/.local/bin/samtools` exist? (No)
2. Does `/usr/local/bin/samtools` exist? (Yes $\to$ executes this binary and stops searching).

If the binary is not in any folder listed in `$PATH`, the shell reports: `command not found`.

### What are `.bashrc` and `.zshrc`?
These are hidden configuration scripts ("dotfiles") stored in your home directory (`~`):
* **`.bashrc`**: Executed automatically every time you start a new **Bash** shell.
* **`.zshrc`**: Executed automatically every time you start a new **Zsh** shell (the default on macOS).

Whenever you install custom bioinformatics software, Conda environments, or custom scripts, you add their directory locations to `$PATH` inside your `.bashrc` or `.zshrc` so they are permanently available:

```bash
# Appending a custom directory to PATH in ~/.bashrc
export PATH="$HOME/tools/subread/bin:$PATH"

# Creating persistent command shortcuts (aliases)
alias ll="ls -lah"
```

To apply changes immediately to your current session without logging out:
```bash
source ~/.bashrc
```

---

## 6. High-Performance Computing (HPC) & Batch Schedulers (SLURM / PBS)

On a standalone server, you can run commands directly on the terminal. However, an institutional **HPC cluster** is shared among hundreds of researchers simultaneously.

```
                                [ Institutional HPC Cluster ]
                                              |
                   +--------------------------+--------------------------+
                   |                                                     |
          [ Login / Head Node ]                                [ Compute Nodes ]
          - Shared entry point                                 - 50 to 1,000+ nodes
          - Light tasks ONLY (editing, git, queuing)           - Massive CPU / RAM / GPUs
          - NO heavy computation allowed                       - Isolated job execution
                   |                                                     ^
                   | Submits Batch Script                                |
                   v                                                     |
          [ Resource Scheduler ] ----------------------------------------+
          (SLURM / PBS)          Allocates matched cores & RAM when free
```

### Cluster Node Specialization
In cluster computing, a **node** is an individual physical computer (or distinct virtual machine) connected to the shared high-speed network. An HPC cluster organizes its nodes into specialized functional roles:
* **Login Node (Head Node)**: The single entry-point computer that users connect to via SSH. It is a shared gateway meant strictly for editing scripts, managing files, and submitting jobs.
  * *Why heavy computation is prohibited on login nodes*: If one user launches a 32-thread genome alignment on the login node, it consumes 100% of the CPU and RAM, freezing the terminal for all other researchers trying to log in. Systems administrators automatically terminate unqueued compute jobs on login nodes.
* **Compute Nodes**: The heavy worker computers (often dozens to hundreds in a cluster) equipped with massive CPU core counts (64–256 cores), terabytes of RAM, and GPUs. These nodes are isolated from direct user logins and execute jobs submitted via the scheduler.
* **Storage Nodes / Shared Filesystem**: Centralized storage servers (e.g., Lustre, NFS) mounted across all nodes so files are accessible everywhere on the cluster.

### What are Schedulers (SLURM / PBS)?
Schedulers like **SLURM** (Simple Linux Utility for Resource Management) and **PBS** (Portable Batch System) act as automated air-traffic controllers for cluster hardware. They match incoming user resource requests to available compute nodes.

### How Resource Allocation and Queuing Work
To run a computation, you write a **batch submission script** declaring the exact hardware resources your job requires:

```bash
#!/bin/bash
#SBATCH --job-name=align_sample1      # Human-readable job name
#SBATCH --partition=compute           # Queue partition to submit to
#SBATCH --nodes=1                     # Request 1 physical server node
#SBATCH --cpus-per-task=16            # Request 16 CPU execution threads
#SBATCH --mem=64G                     # Request 64 GB of physical RAM
#SBATCH --time=12:00:00               # Hard runtime limit (12 hours)
#SBATCH --output=logs/align_%j.out    # Redirect stdout to log file (%j = Job ID)
#SBATCH --error=logs/align_%j.err     # Redirect stderr to error file

# Load software environment module
module load samtools/1.19

# Run the computational command
bwa mem -t 16 /ref/hg38.fa reads_1.fq reads_2.fq | samtools sort -@ 16 -o sample1.bam
```

### The Job Lifecycle:
1. **Submission (`sbatch script.sh`)**: The script is sent to the scheduler daemon.
2. **Pending in Queue**: The scheduler places the job in a queue. It evaluates priority based on **fair-share** algorithms (how many resources your lab has used recently) and requested parameters (smaller jobs often get scheduled faster than massive jobs).
3. **Allocation & Execution**: Once a compute node with 16 free CPU cores and 64 GB of free RAM becomes available, the scheduler reserves those hardware resources, copies environment variables, runs the script in isolation on that node, and streams output to `align_<job_id>.out`.
4. **Monitoring**: You track your queued and running jobs using `squeue -u $USER`.
5. **Completion**: When the command finishes (or if it hits the `--time` limit or `--mem` OOM limit), the scheduler releases the compute node hardware back to the cluster pool for the next researcher.

> **Summary**: Remote computing scales computational capabilities beyond the laptop. SSH provides encrypted terminal tunnels, `tmux` guarantees process survival against network drops, `$PATH` and `.bashrc` structure executable environments, and batch schedulers (SLURM/PBS) manage fair access to massive shared compute nodes.

<div class="page-nav">
  <a href="operating-systems.html" class="page-nav-prev">&larr; Operating Systems</a>
  <a href="../cs101/index.html" class="page-nav-next">Next Course: CS101 &rarr;</a>
</div>

