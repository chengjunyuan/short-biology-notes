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

<!-- Server Physical Architecture & Networking Pipeline Schematic -->
<svg viewBox="0 0 780 280" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue-hpc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
    <marker id="arrow-emerald-hpc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="280" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left: Data Center Rack -->
  <g transform="translate(20, 20)">
    <rect width="255" height="240" rx="8" fill="#ffffff" stroke="#475569" stroke-width="1.5"/>
    <rect width="255" height="28" rx="8" fill="#1e293b"/>
    <text x="127" y="19" text-anchor="middle" font-size="11.5" font-weight="700" fill="#f8fafc">Data Center Rack (Headless Nodes)</text>

    <!-- Rack Unit 1: Compute Node -->
    <g transform="translate(10, 36)">
      <rect width="235" height="58" rx="5" fill="#f8fafc" stroke="#94a3b8" stroke-width="1"/>
      <text x="10" y="18" font-size="10.5" font-weight="700" fill="#1e293b">1U Compute Node (Worker)</text>
      <text x="10" y="34" font-size="9.5" fill="#475569">• 128 Cores (Dual EPYC/Xeon)</text>
      <text x="10" y="48" font-size="9.5" fill="#64748b">• 512 GB – 1.5 TB DDR5 RAM</text>
    </g>

    <!-- Rack Unit 2: GPU Node -->
    <g transform="translate(10, 101)">
      <rect width="235" height="58" rx="5" fill="#f0fdf4" stroke="#86efac" stroke-width="1"/>
      <text x="10" y="18" font-size="10.5" font-weight="700" fill="#166534">4U Accelerator / AI Node</text>
      <text x="10" y="34" font-size="9.5" fill="#15803d">• 8× NVIDIA H100 GPUs (640 GB)</text>
      <text x="10" y="48" font-size="9.5" fill="#15803d">• Cryo-EM &amp; Deep Learning</text>
    </g>

    <!-- Rack Unit 3: Storage Array -->
    <g transform="translate(10, 166)">
      <rect width="235" height="64" rx="5" fill="#eff6ff" stroke="#93c5fd" stroke-width="1"/>
      <text x="10" y="18" font-size="10.5" font-weight="700" fill="#1e40af">Storage Array (Lustre / GPFS)</text>
      <text x="10" y="34" font-size="9.5" fill="#1d4ed8">• Petabytes NVMe + High-Cap HDD</text>
      <text x="10" y="48" font-size="9.5" fill="#64748b">• Shared across cluster nodes</text>
    </g>
  </g>

  <!-- Middle-Left: Internal High-Speed Cluster Fabric -->
  <g transform="translate(275, 0)">
    <path d="M 7 130 L 73 130" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-emerald-hpc)"/>
    <path d="M 73 146 L 7 146" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-emerald-hpc)"/>
    <text x="40" y="116" text-anchor="middle" font-size="9" font-weight="700" fill="#059669">10–100 Gbps</text>
    <text x="40" y="165" text-anchor="middle" font-size="8.5" font-weight="600" fill="#047857">Cluster Fabric</text>
    <text x="40" y="177" text-anchor="middle" font-size="8" fill="#64748b">(Low Latency)</text>
  </g>

  <!-- Middle: Institutional Gateway Box -->
  <g transform="translate(355, 30)">
    <rect width="120" height="220" rx="7" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <rect width="120" height="26" rx="7" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="60" y="17" text-anchor="middle" font-size="10.5" font-weight="700" fill="#6d28d9">Campus Gateway</text>

    <!-- Security Firewall -->
    <rect x="8" y="35" width="104" height="42" rx="4" fill="#faf5ff" stroke="#ddd6fe"/>
    <text x="60" y="50" text-anchor="middle" font-size="9" font-weight="700" fill="#6d28d9">Security Firewall</text>
    <text x="60" y="63" text-anchor="middle" font-size="8" fill="#475569">Blocks raw ports</text>
    <text x="60" y="73" text-anchor="middle" font-size="8" fill="#64748b">Inspects traffic</text>

    <!-- Bastion / Login -->
    <rect x="8" y="85" width="104" height="52" rx="4" fill="#faf5ff" stroke="#ddd6fe"/>
    <text x="60" y="100" text-anchor="middle" font-size="9" font-weight="700" fill="#6d28d9">Bastion / Head</text>
    <text x="60" y="113" text-anchor="middle" font-size="8" fill="#475569">VPN / Login Node</text>
    <text x="60" y="125" text-anchor="middle" font-size="8.5" font-weight="700" fill="#7c3aed">Port 22 (SSH)</text>

    <!-- Slurm / Internal Routing -->
    <rect x="8" y="145" width="104" height="65" rx="4" fill="#faf5ff" stroke="#ddd6fe"/>
    <text x="60" y="160" text-anchor="middle" font-size="9" font-weight="700" fill="#6d28d9">Cluster Router</text>
    <text x="60" y="173" text-anchor="middle" font-size="8" fill="#475569">Slurm Controller</text>
    <text x="60" y="185" text-anchor="middle" font-size="8" fill="#475569">Job Scheduling</text>
    <text x="60" y="197" text-anchor="middle" font-size="8" fill="#64748b">&amp; Node Routing</text>
  </g>

  <!-- Middle-Right: Encrypted SSH / VPN Connection to Client -->
  <g transform="translate(475, 0)">
    <path d="M 7 130 L 73 130" stroke="#2563eb" stroke-width="2.5" stroke-dasharray="5,3" marker-end="url(#arrow-blue-hpc)"/>
    <path d="M 73 146 L 7 146" stroke="#2563eb" stroke-width="2.5" stroke-dasharray="5,3" marker-end="url(#arrow-blue-hpc)"/>
    <text x="40" y="110" text-anchor="middle" font-size="9" font-weight="700" fill="#2563eb">Encrypted</text>
    <text x="40" y="121" text-anchor="middle" font-size="9" font-weight="700" fill="#2563eb">SSH Tunnel</text>
    <text x="40" y="165" text-anchor="middle" font-size="8.5" font-weight="600" fill="#1d4ed8">Internet / Wi-Fi</text>
    <text x="40" y="177" text-anchor="middle" font-size="8" fill="#64748b">(Text/Key I/O)</text>
  </g>

  <!-- Right: Local Laptop -->
  <g transform="translate(555, 20)">
    <rect width="205" height="240" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="1.5"/>
    <rect width="205" height="28" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="102" y="19" text-anchor="middle" font-size="11.5" font-weight="700" fill="#1d4ed8">Local Laptop / Client</text>

    <text x="12" y="48" font-size="9.5" font-weight="600" fill="#1e293b">• Interactive IDE (VS Code)</text>
    <text x="12" y="64" font-size="9" fill="#475569">• Prototyping small datasets</text>
    <text x="12" y="80" font-size="9" fill="#475569">• Plot &amp; figure inspection</text>
    <text x="12" y="96" font-size="9" fill="#475569">• Terminal &amp; SSH key host</text>

    <!-- Local Constraints Box -->
    <rect x="10" y="112" width="185" height="118" rx="5" fill="#f8fafc" stroke="#cbd5e1"/>
    <text x="18" y="130" font-size="9.5" font-weight="700" fill="#0f172a">Local Constraints:</text>
    <text x="18" y="148" font-size="9" fill="#64748b">• Limited: 8–32 GB RAM</text>
    <text x="18" y="164" font-size="9" fill="#64748b">• 4–16 CPU cores (thermal limits)</text>
    <text x="18" y="180" font-size="9" fill="#64748b">• Sleep/disconnects on lid close</text>
    <text x="18" y="196" font-size="9" fill="#dc2626">• No high-throughput GPUs</text>
    <text x="18" y="212" font-size="9" fill="#64748b">• Battery &amp; consumer Wi-Fi</text>
  </g>
</svg>

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

<!-- SSH Cryptographic Protocol and Tunnel Architecture Schematic -->
<svg viewBox="0 0 780 250" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue-ssh" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
    <marker id="arrow-green-ssh" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="250" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left: Client Machine -->
  <g transform="translate(25, 25)">
    <rect width="220" height="200" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="1.5"/>
    <rect width="220" height="28" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="110" y="19" text-anchor="middle" font-size="12" font-weight="700" fill="#1d4ed8">Local SSH Client (Laptop)</text>
    
    <text x="16" y="52" font-size="11" font-weight="600" fill="#334155">• Terminal Host (Zsh/Bash)</text>
    <text x="16" y="74" font-size="11" fill="#475569">• OpenSSH Client process</text>
    <text x="16" y="96" font-size="11" fill="#475569">• Keystroke capture</text>
    
    <rect x="12" y="112" width="196" height="75" rx="5" fill="#f8fafc" stroke="#cbd5e1"/>
    <text x="20" y="130" font-size="10" font-weight="700" fill="#0f172a">Private Key (Confidential):</text>
    <text x="20" y="146" font-size="9" font-family="monospace" fill="#2563eb">~/.ssh/id_ed25519</text>
    <text x="20" y="162" font-size="9" fill="#64748b">• Stays on laptop</text>
    <text x="20" y="176" font-size="9" fill="#64748b">• Signs crypto challenges</text>
  </g>

  <!-- Middle: Encrypted Network Tunnel -->
  <g transform="translate(265, 30)">
    <rect width="250" height="190" rx="8" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="125" y="24" text-anchor="middle" font-size="12" font-weight="700" fill="#6d28d9">Encrypted TCP Port 22 Tunnel</text>

    <!-- Top Arrow: Keystrokes -->
    <path d="M 15 55 L 235 55" stroke="#2563eb" stroke-width="2.5" marker-end="url(#arrow-blue-ssh)"/>
    <text x="125" y="48" text-anchor="middle" font-size="10" font-weight="600" fill="#2563eb">Keystrokes / Commands Encrypted ➔</text>

    <!-- Middle Cryptographic Badge -->
    <g transform="translate(20, 75)">
      <rect width="210" height="48" rx="6" fill="#ffffff" stroke="#c4b5fd"/>
      <text x="105" y="20" text-anchor="middle" font-size="10" font-weight="700" fill="#5b21b6">Asymmetric Cryptography</text>
      <text x="105" y="36" text-anchor="middle" font-size="9" fill="#6d28d9">End-to-end AES-256 / ChaCha20 cipher</text>
    </g>

    <!-- Bottom Arrow: stdout/stderr -->
    <path d="M 235 155 L 15 155" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-green-ssh)"/>
    <text x="125" y="148" text-anchor="middle" font-size="10" font-weight="600" fill="#059669">⬅ stdout / stderr Stream Returned</text>
  </g>

  <!-- Right: Remote Server -->
  <g transform="translate(535, 25)">
    <rect width="220" height="200" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="220" height="28" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="110" y="19" text-anchor="middle" font-size="12" font-weight="700" fill="#047857">Remote Server (Headless)</text>
    
    <text x="16" y="52" font-size="11" font-weight="600" fill="#334155">• SSH Daemon (<tspan font-family="monospace">sshd</tspan>)</text>
    <text x="16" y="74" font-size="11" fill="#475569">• Allocates Pseudo-terminal</text>
    <text x="16" y="96" font-size="11" fill="#475569">• Executes commands on CPU</text>
    
    <rect x="12" y="112" width="196" height="75" rx="5" fill="#f8fafc" stroke="#cbd5e1"/>
    <text x="20" y="130" font-size="10" font-weight="700" fill="#0f172a">Public Key (Authorized):</text>
    <text x="20" y="146" font-size="9" font-family="monospace" fill="#047857">~/.ssh/authorized_keys</text>
    <text x="20" y="162" font-size="9" fill="#64748b">• Deployed to server</text>
    <text x="20" y="176" font-size="9" fill="#64748b">• Validates client signatures</text>
  </g>
</svg>

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

<!-- tmux Session Persistence vs SIGHUP Failure Schematic -->
<svg viewBox="0 0 780 300" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-red-tmux" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#dc2626" />
    </marker>
    <marker id="arrow-green-tmux" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="300" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Top: The Failure Mode (Raw SSH Session) -->
  <g transform="translate(25, 18)">
    <rect width="730" height="118" rx="8" fill="#ffffff" stroke="#ef4444" stroke-width="1.5"/>
    <rect width="730" height="24" rx="8" fill="#fee2e2" stroke="#ef4444" stroke-width="1.5"/>
    <text x="365" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#991b1b">Failure Mode: Raw SSH Session (No Terminal Multiplexer)</text>

    <g transform="translate(15, 36)">
      <!-- Step 1 -->
      <rect x="0" y="0" width="150" height="64" rx="5" fill="#fef2f2" stroke="#fca5a5"/>
      <text x="75" y="24" text-anchor="middle" font-size="11" font-weight="700" fill="#991b1b">Direct SSH Process</text>
      <text x="75" y="44" text-anchor="middle" font-size="9" fill="#7f1d1d">Job tied to TCP pipe</text>

      <!-- Arrow 1 -->
      <path d="M 155 32 L 180 32" stroke="#dc2626" stroke-width="2" marker-end="url(#arrow-red-tmux)"/>

      <!-- Step 2 -->
      <rect x="185" y="0" width="155" height="64" rx="5" fill="#fee2e2" stroke="#f87171"/>
      <text x="262" y="24" text-anchor="middle" font-size="11" font-weight="700" fill="#991b1b">Wi-Fi Drops / Sleep</text>
      <text x="262" y="44" text-anchor="middle" font-size="9" fill="#7f1d1d">SSH connection dies</text>

      <!-- Arrow 2 -->
      <path d="M 345 32 L 370 32" stroke="#dc2626" stroke-width="2" marker-end="url(#arrow-red-tmux)"/>

      <!-- Step 3 -->
      <rect x="375" y="0" width="155" height="64" rx="5" fill="#fee2e2" stroke="#f87171"/>
      <text x="452" y="24" text-anchor="middle" font-size="11" font-weight="700" fill="#991b1b">Kernel Emits SIGHUP</text>
      <text x="452" y="44" text-anchor="middle" font-size="9" fill="#7f1d1d">Signal 1: Terminal Hangup</text>

      <!-- Arrow 3 -->
      <path d="M 535 32 L 560 32" stroke="#dc2626" stroke-width="2" marker-end="url(#arrow-red-tmux)"/>

      <!-- Step 4 -->
      <rect x="565" y="0" width="135" height="64" rx="5" fill="#dc2626"/>
      <text x="632" y="28" text-anchor="middle" font-size="11" font-weight="700" fill="#ffffff">Process Killed!</text>
      <text x="632" y="46" text-anchor="middle" font-size="9" fill="#fecaca">14h alignment lost</text>
    </g>
  </g>

  <!-- Bottom: The Solution (tmux Persistence) -->
  <g transform="translate(25, 148)">
    <rect width="730" height="135" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="730" height="24" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="365" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#047857">The Solution: tmux Background Server Daemon</text>

    <g transform="translate(15, 34)">
      <!-- Box 1: Laptop Day 1 -->
      <rect x="0" y="0" width="150" height="82" rx="5" fill="#f0fdf4" stroke="#86efac"/>
      <text x="75" y="22" text-anchor="middle" font-size="11" font-weight="700" fill="#166534">Day 1: Start Session</text>
      <text x="75" y="40" text-anchor="middle" font-size="9" font-family="monospace" fill="#065f46">$ tmux new -s align</text>
      <text x="75" y="56" text-anchor="middle" font-size="9" fill="#15803d">Launch alignment job</text>
      <text x="75" y="70" text-anchor="middle" font-size="9" font-weight="600" fill="#166534">Detach (Ctrl+B, D)</text>

      <!-- Arrow to Server Daemon -->
      <path d="M 155 41 L 180 41" stroke="#059669" stroke-width="2" marker-end="url(#arrow-green-tmux)"/>

      <!-- Box 2: tmux Server Daemon on Remote Machine -->
      <rect x="185" y="0" width="345" height="82" rx="5" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
      <text x="357" y="22" text-anchor="middle" font-size="11" font-weight="700" fill="#047857">tmux Remote Server Daemon (Persistent)</text>
      <text x="357" y="42" text-anchor="middle" font-size="10" fill="#065f46">Allocates isolated pseudo-terminal (PTY) independent of SSH</text>
      <rect x="200" y="52" width="315" height="22" rx="4" fill="#d1fae5"/>
      <text x="357" y="67" text-anchor="middle" font-size="10" font-weight="600" fill="#047857">Job runs 24/7 continuously even if Wi-Fi drops or laptop sleeps</text>

      <!-- Arrow to Day 2 Reattach -->
      <path d="M 535 41 L 560 41" stroke="#059669" stroke-width="2" marker-end="url(#arrow-green-tmux)"/>

      <!-- Box 3: Laptop Day 2 -->
      <rect x="565" y="0" width="135" height="82" rx="5" fill="#f0fdf4" stroke="#86efac"/>
      <text x="632" y="22" text-anchor="middle" font-size="11" font-weight="700" fill="#166534">Day 2: Reattach</text>
      <text x="632" y="40" text-anchor="middle" font-size="9" font-family="monospace" fill="#065f46">$ tmux attach -t align</text>
      <text x="632" y="58" text-anchor="middle" font-size="9" fill="#15803d">Terminal state restored</text>
      <text x="632" y="72" text-anchor="middle" font-size="9" font-weight="700" fill="#047857">✓ 100% Completed</text>
    </g>
  </g>
</svg>

### The Solution: `tmux` (Terminal Multiplexer)
**`tmux`** runs a persistent server daemon *on the remote machine itself*, detaching your programs from the fragile SSH connection.

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

<!-- HPC Cluster Topology and Batch Scheduler Schematic -->
<svg viewBox="0 0 780 300" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-purple-slurm" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#7c3aed" />
    </marker>
    <marker id="arrow-green-slurm" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="300" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left: User & Login Node -->
  <g transform="translate(20, 20)">
    <rect width="220" height="260" rx="8" fill="#ffffff" stroke="#f59e0b" stroke-width="1.5"/>
    <rect width="220" height="28" rx="8" fill="#fef3c7" stroke="#f59e0b" stroke-width="1.5"/>
    <text x="110" y="19" text-anchor="middle" font-size="12" font-weight="700" fill="#92400e">Login Node (Head Node)</text>

    <text x="14" y="54" font-size="11" font-weight="600" fill="#334155">• Shared Entry Gateway</text>
    <text x="14" y="74" font-size="11" fill="#475569">• Light tasks ONLY</text>
    <text x="14" y="94" font-size="11" fill="#475569">• File editing (<tspan font-family="monospace">vim</tspan>, VS Code)</text>
    <text x="14" y="114" font-size="11" fill="#475569">• Git sync &amp; script preparation</text>
    <text x="14" y="134" font-size="11" font-family="monospace" fill="#7c3aed">• sbatch submit_job.sh</text>

    <rect x="10" y="152" width="200" height="96" rx="5" fill="#fef2f2" stroke="#fca5a5"/>
    <text x="104" y="172" text-anchor="middle" font-size="11" font-weight="700" fill="#991b1b">⚠️ STRICT RULE</text>
    <text x="106" y="190" text-anchor="middle" font-size="10" fill="#b91c1c">NO heavy computation here!</text>
    <text x="106" y="206" text-anchor="middle" font-size="9" fill="#7f1d1d">Heavy jobs freeze the node for</text>
    <text x="106" y="220" text-anchor="middle" font-size="9" fill="#7f1d1d">all researchers &amp; get killed.</text>
  </g>

  <!-- Middle: Resource Scheduler (SLURM / PBS) -->
  <g transform="translate(270, 20)">
    <!-- Arrow from Login Node to SLURM -->
    <path d="M -27 80 L -7 80" stroke="#7c3aed" stroke-width="2.5" marker-end="url(#arrow-purple-slurm)"/>

    <rect width="210" height="260" rx="8" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <rect width="210" height="28" rx="8" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="105" y="19" text-anchor="middle" font-size="12" font-weight="700" fill="#6d28d9">Scheduler (SLURM / PBS)</text>

    <text x="14" y="54" font-size="11" font-weight="700" fill="#4338ca">Queue &amp; Allocator</text>
    <text x="14" y="74" font-size="10" fill="#334155">1. Inspects job requirements:</text>
    <text x="24" y="90" font-size="10" font-family="monospace" fill="#6d28d9">--cpus=16, --mem=64G</text>
    
    <text x="14" y="114" font-size="10" fill="#334155">2. Evaluates Priority:</text>
    <text x="24" y="130" font-size="10" fill="#475569">• Fair-share policy</text>
    <text x="24" y="146" font-size="10" fill="#475569">• Partition wait times</text>

    <text x="14" y="172" font-size="10" fill="#334155">3. Dispatches job to</text>
    <text x="24" y="188" font-size="10" fill="#334155">matched worker node</text>

    <rect x="10" y="206" width="190" height="42" rx="4" fill="#ede9fe"/>
    <text x="105" y="224" text-anchor="middle" font-size="10" font-weight="700" fill="#5b21b6">Resource Isolation</text>
    <text x="105" y="238" text-anchor="middle" font-size="9" fill="#6d28d9">Enforces memory &amp; CPU limits</text>
  </g>

  <!-- Right: Compute Nodes Pool -->
  <g transform="translate(510, 20)">
    <!-- Arrow from SLURM to Compute Pool -->
    <path d="M -27 80 L -7 80" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-green-slurm)"/>

    <rect width="250" height="260" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="250" height="28" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="125" y="19" text-anchor="middle" font-size="12" font-weight="700" fill="#047857">Compute Nodes Pool (50–1,000+)</text>

    <!-- Node 1 -->
    <g transform="translate(12, 38)">
      <rect width="226" height="58" rx="5" fill="#f0fdf4" stroke="#86efac"/>
      <text x="10" y="18" font-size="11" font-weight="700" fill="#166534">Compute Node 01 [Executing]</text>
      <text x="10" y="34" font-size="10" fill="#15803d">• Running: <tspan font-family="monospace">bwa mem -t 16</tspan></text>
      <text x="10" y="48" font-size="9" fill="#64748b">• Allocated: 16 Cores, 64 GB RAM</text>
    </g>

    <!-- Node 2 -->
    <g transform="translate(12, 102)">
      <rect width="226" height="58" rx="5" fill="#f0fdf4" stroke="#86efac"/>
      <text x="10" y="18" font-size="11" font-weight="700" fill="#166534">Compute Node 02 [Executing]</text>
      <text x="10" y="34" font-size="10" fill="#15803d">• Running: AlphaFold3 Prediction</text>
      <text x="10" y="48" font-size="9" fill="#64748b">• Allocated: 4× H100 GPUs, 256 GB RAM</text>
    </g>

    <!-- Shared Storage Bar -->
    <g transform="translate(12, 168)">
      <rect width="226" height="80" rx="5" fill="#eff6ff" stroke="#93c5fd"/>
      <text x="113" y="20" text-anchor="middle" font-size="11" font-weight="700" fill="#1e40af">Shared Cluster Filesystem</text>
      <text x="113" y="38" text-anchor="middle" font-size="10" fill="#1d4ed8">/scratch, /home, /data</text>
      <text x="113" y="56" text-anchor="middle" font-size="9" fill="#64748b">Unified high-speed parallel access</text>
      <text x="113" y="70" text-anchor="middle" font-size="9" fill="#64748b">across all compute nodes</text>
    </g>
  </g>
</svg>

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

