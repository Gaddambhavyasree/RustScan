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

<img width="516" height="289" alt="image" src="https://github.com/user-attachments/assets/ca77af68-61f1-4852-abb4-bbb3bcb364c4" />

# Basic Usage

## Scan a Host

```bash
rustscan -a 10.0.2.15
```


Example Output

<img width="752" height="648" alt="Screenshot 2026-07-09 222423" src="https://github.com/user-attachments/assets/ae486249-8d8f-4141-b208-d1f2d30baf38" />

---

# Service Detection

```bash
rustscan -a 10.0.2.15 -- -sV
```

Example Result

<img width="758" height="403" alt="Screenshot 2026-07-09 222649" src="https://github.com/user-attachments/assets/046fcb1c-4946-4ecf-874f-9d3a9f616973" />

<img width="757" height="251" alt="Screenshot 2026-07-09 222733" src="https://github.com/user-attachments/assets/d8dbac0c-cea4-41c9-9611-3e8586d54f69" />

**Purpose**

- Detect running services
- Identify software versions

---

# Run Default NSE Scripts

```bash
rustscan -a 10.0.2.15 -- -sC
```

Example Result

<img width="677" height="517" alt="Screenshot 2026-07-09 222824" src="https://github.com/user-attachments/assets/4aa8d080-990f-4eb1-b6b5-d53e2c2619c3" />


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

<img width="756" height="662" alt="Screenshot 2026-07-09 222949" src="https://github.com/user-attachments/assets/852ead08-6214-4ded-8985-8005ebbe98fe" />


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

<img width="752" height="602" alt="Screenshot 2026-07-09 223050" src="https://github.com/user-attachments/assets/bd7caeac-d8ed-4d0f-9817-59220501e7ba" />


---

Scan all ports

```bash
rustscan -a 10.0.2.15 -r 1-65535
```

Output:

<img width="752" height="658" alt="Screenshot 2026-07-09 223124" src="https://github.com/user-attachments/assets/336fc93f-0576-400f-89c6-7eec9846a0c7" />


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
# Disclaimer

This is strictly for educational purposes. All demonstrations, commands done in my own lab environment. 
Do not try against systems that you do not own.
The author is not responsible for any misue or illegal Activities.
Follow the laws and ethical Guidelines.

# Thankyou.







