# RustScan Installation and Usage on Kali Linux

## Overview

**RustScan** is a high-speed TCP port scanner written in Rust. It rapidly discovers open TCP ports and then hands the results to **Nmap** for detailed service detection, version detection, OS fingerprinting, and NSE script execution.

## Why RustScan?

- ⚡ Extremely fast TCP port scanning
- 🔍 Integrates seamlessly with Nmap
- 🦀 Written in Rust
- 🎯 Ideal for the reconnaissance phase of authorized security assessments

---

# Installation

## Attempt using APT

```bash
sudo apt install rustscan
```

Output:

```text
Error: Unable to locate package rustscan
```

RustScan was not available in the default Kali repositories.

---

## Install using Cargo

Install RustScan using Cargo:

```bash
cargo install rustscan
```

Successful installation:

```text
Installed package `rustscan v2.4.1`
```

---

## Verify the Shell

Check which shell is running:

```bash
echo $SHELL
```

Output:

```text
/usr/bin/zsh
```

---

## Add RustScan to PATH

Since Kali was using **Zsh**, add Cargo's binary directory to `.zshrc`.

```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
```

Reload the configuration:

```bash
source ~/.zshrc
```

---

## Verify Installation

```bash
rustscan --version
```

Output:

```text
rustscan 2.4.1
```

Locate the executable:

```bash
which rustscan
```

Output:

```text
/home/kali/.cargo/bin/rustscan
```

---

# Basic Usage

## Scan a Host

```bash
rustscan -a 10.0.2.15
```

Example Output

```text
Open 10.0.2.15:4000
```

RustScan automatically launches Nmap against the discovered port.

---

# Service Detection

```bash
rustscan -a 10.0.2.15 -- -sV
```

Example Result

```text
4000/tcp open http Golang net/http server
```

**Purpose**

- Detect running services
- Identify software versions

---

# Run Default NSE Scripts

```bash
rustscan -a 10.0.2.15 -- -sC
```

Runs Nmap's default NSE scripts against discovered ports.

---

# Aggressive Scan

```bash
rustscan -a 10.0.2.15 -- -A
```

Performs:

- Service Detection
- Version Detection
- OS Detection
- Default NSE Scripts
- Traceroute (where applicable)

Example findings:

- HTTP Service
- Golang net/http server
- Linux Kernel Fingerprint

---

# Scan a Specific Port

```bash
rustscan -a 10.0.2.15 -p 80
```

No open port was found because the service was not running on port 80.

---

# Scan a Port Range

Ports **1–1000**

```bash
rustscan -a 10.0.2.15 -r 1-1000
```

No results because the service was running on **port 4000**.

---

Ports **1–4000**

```bash
rustscan -a 10.0.2.15 -r 1-4000
```

Output:

```text
Open 4000
```

---

Scan all ports

```bash
rustscan -a 10.0.2.15 -r 1-65535
```

Output:

```text
Open 4000
```

---

# Understanding `--`

Everything after `--` is passed directly to **Nmap**.

Example:

```bash
rustscan -a 10.0.2.15 -- -sV
```

Workflow:

```
RustScan
        │
        ▼
Discovers Open Ports
        │
        ▼
Runs Nmap
        │
        ▼
Service Detection
Version Detection
OS Detection
NSE Scripts
```

---

# Common Commands

| Command | Description |
|----------|-------------|
| `rustscan -a <IP>` | Basic scan |
| `rustscan -a <IP> -- -sV` | Service detection |
| `rustscan -a <IP> -- -sC` | Default NSE scripts |
| `rustscan -a <IP> -- -A` | Aggressive scan |
| `rustscan -a <IP> -p 80` | Scan a specific port |
| `rustscan -a <IP> -r 1-1000` | Scan a port range |
| `rustscan -a <IP> -r 1-65535` | Scan all TCP ports |

---

# Notes

RustScan displayed the following warning:

```text
File limit is lower than default batch size.
```

This means the operating system's file descriptor limit (`ulimit`) is lower than RustScan's default batch size.

Performance can be improved by increasing the file descriptor limit:

```bash
ulimit -n 5000
```

or

```bash
rustscan --ulimit 5000 -a <target>
```

---



## Lab Environment

- Operating System: Kali Linux
- Shell: Zsh
- RustScan Version: 2.4.1
- Scanner: RustScan + Nmap
