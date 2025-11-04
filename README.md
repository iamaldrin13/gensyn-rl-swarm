# RL Swarm - Linux Installation Guide

## Overview

**RL Swarm** is a peer-to-peer system for reinforcement learning that allows you to train models collaboratively over the internet. It's powered by the [GenRL](https://github.com/gensyn-ai/genrl) library and connects to the Gensyn Testnet for on-chain identity tracking.

**Current Swarm**: Training models on the [reasoning-gym](https://github.com/open-thought/reasoning-gym) dataset with models ranging from 0.5B to 1.7B parameters.

---

## System Requirements

### CPU-Only Setup
- **Processor**: arm64 or x86 CPU
- **RAM**: Minimum 32GB (avoid running other heavy applications during training)
- **Python**: 3.10 or higher

### GPU Setup (Officially Supported)
- **NVIDIA GPUs**: RTX 3090, RTX 4090, RTX 5090, A100, H100
- **CUDA**: Required for GPU acceleration
- **RAM**: 32GB+ recommended
- **Python**: 3.10 or higher

---

## Quick Installation (Linux)

### Part 1: System Setup & Tailscale Authentication

Run these commands first:

```bash
apt update && apt install -y sudo
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof nano unzip iproute2 build-essential gcc g++
curl -fsSL https://tailscale.com/install.sh | sh
git clone https://github.com/gensyn-ai/rl-swarm.git && cd rl-swarm
tailscale up --ssh
```

**⚠️ STOP HERE - Login to Tailscale First!**

After running `tailscale up --ssh`, you'll receive a URL like:
```
To authenticate, visit: https://login.tailscale.com/a/abc123def456
```

1. Open the URL in your browser
2. Login with your Tailscale account
3. Authorize the device
4. Wait for confirmation: "Success" message in terminal

**Once Tailscale login is successful, proceed to Part 2.**

---

### Part 2: Launch RL Swarm

After completing Tailscale authentication, run:

```bash
tailscale serve --bg http://localhost:3000
python3 -m venv .venv && source .venv/bin/activate && bash run_rl_swarm.sh
```

---

## Installation Breakdown

### Step 1: Update Package Manager

```bash
apt update && apt install -y sudo
```

**What it does:**
- `apt update` - Refreshes package lists from repositories
- `apt install -y sudo` - Installs sudo for privilege escalation (auto-confirms with -y flag)

### Step 2: Install System Dependencies

```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof nano unzip iproute2 build-essential gcc g++
```

**What it does:**
- `python3` - Python interpreter (required for running RL Swarm)
- `python3-venv` - Virtual environment support for isolated Python packages
- `python3-pip` - Python package installer
- `curl` / `wget` - Download utilities for fetching files
- `screen` - Terminal multiplexer for persistent sessions
- `git` - Version control for cloning the repository
- `lsof` - Lists open files and network connections (useful for debugging)
- `nano` - Text editor for configuration files
- `unzip` - Archive extraction utility
- `iproute2` - Network configuration tools
- `build-essential` / `gcc` / `g++` - C/C++ compilers for building Python packages with native extensions

### Step 3: Install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

**What it does:**
- Downloads and executes Tailscale's official installation script
- `curl -fsSL` flags: fail silently on errors (-f), silent mode (-s), show errors (-S), follow redirects (-L)
- Tailscale creates a secure mesh VPN network for accessing your node remotely

### Step 4: Clone RL Swarm Repository

```bash
git clone https://github.com/gensyn-ai/rl-swarm.git && cd rl-swarm
```

**What it does:**
- Clones the rl-swarm GitHub repository to your local machine
- Changes directory into the cloned repository
- The `&&` operator ensures the second command only runs if cloning succeeds

### Step 5: Connect to Tailscale Network

```bash
tailscale up --ssh
```

**What it does:**
- `tailscale up` - Authenticates and connects your machine to your Tailscale network
- `--ssh` - Enables SSH access over Tailscale, allowing secure remote access to this machine
- **Important**: This command will prompt you to authenticate via a browser URL

**⚠️ CHECKPOINT: You must complete Tailscale authentication before proceeding!**

Expected output:
```
To authenticate, visit:
    https://login.tailscale.com/a/abc123def456
```

**Action Required:**
1. Copy the authentication URL from your terminal
2. Open it in any browser (phone, laptop, etc.)
3. Login to your Tailscale account
4. Authorize this device
5. Wait for "Success" or similar confirmation in terminal
6. **Only then proceed to Step 6**

### Step 6: Expose Local Web Server

```bash
tailscale serve --bg http://localhost:3000
```

**What it does:**
- Creates a Tailscale-accessible endpoint for the RL Swarm login interface (port 3000)
- `--bg` - Runs the serve process in the background
- This allows you to access the login screen from any device on your Tailscale network

### Step 7: Launch RL Swarm

```bash
python3 -m venv .venv && source .venv/bin/activate && bash run_rl_swarm.sh
```

**What it does:**
- `python3 -m venv .venv` - Creates a virtual environment named `.venv` in the current directory
- `source .venv/bin/activate` - Activates the virtual environment (isolates Python packages)
- `bash run_rl_swarm.sh` - Executes the RL Swarm startup script

---

## Tailscale Account Setup

Tailscale is required for secure remote access to your RL Swarm node's login interface.

### Creating a Tailscale Account

1. **Visit Tailscale**: Go to [https://tailscale.com/](https://tailscale.com/)

2. **Sign Up**: Click "Get Started" or "Sign Up"
   - Choose your preferred login method:
     - Google Account
     - Microsoft Account
     - GitHub Account
     - Apple ID
     - Email (SSO)

3. **Complete Registration**: Follow the prompts to verify your account

4. **Download Tailscale Client** (Optional for management):
   - Available for: Windows, macOS, Linux, iOS, Android
   - Useful for managing your Tailscale network from other devices

### Authenticating Your Linux Server

When you run `tailscale up --ssh`, you'll receive output similar to:

```
To authenticate, visit:

    https://login.tailscale.com/a/abc123def456
```

**Steps:**
1. Copy the URL from your terminal
2. Open it in a browser (on any device)
3. Log in with your Tailscale account
4. Authorize the device with a friendly name
5. Your server will automatically connect to your Tailscale network

**Accessing Your Node:**
- Once connected, your node gets a Tailscale IP (e.g., `100.x.x.x`)
- Access the login interface from any Tailscale-connected device: `http://100.x.x.x:3000`

---

## Running the Application

Once installation is complete, the application will:

1. **Open Login Screen**: A browser window should pop up at `http://localhost:3000`
   - If on a VM, manually navigate to this URL from a device on your Tailscale network

2. **Login Prompt**: Click "Login" and authenticate with:
   - Google
   - Email
   - Other supported OAuth providers

3. **Hugging Face Token** (Optional): If you want to upload trained models to Hugging Face, provide your access token when prompted
   - Generate token at: [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)

4. **AI Prediction Market** (Optional): Choose whether to participate in the prediction market experiment
   - Press `ENTER` or `Y` to join (default)
   - Press `N` to opt out

5. **Training Begins**: Your device will start training and register on-chain
   - Monitor progress: [https://dashboard.gensyn.ai/](https://dashboard.gensyn.ai/)
   - View transactions: [Gensyn Testnet Explorer](https://gensyn-testnet.explorer.alchemy.com/address/0xFaD7C5e93f28257429569B854151A1B8DCD404c2?tab=logs)

---

## Identity Management

### How It Works

- **On-Chain Identity**: Managed via Alchemy modal sign-in
- **swarm.pem**: Local file linking your peer to your email/EOA
- **Session Keys**: Stored in `userApiKey` (not your EOA keys)

### Multiple Nodes

To run multiple nodes under one account:
1. Use the **same email address** for each node
2. Each node generates its own `swarm.pem`
3. All nodes link to the same on-chain EOA

### Important Rules

✅ **Works:**
- First run with new email + new `swarm.pem`
- Re-running with same `swarm.pem` + original email

❌ **Doesn't Work:**
- Keeping `swarm.pem` but changing to different email

### Troubleshooting Identity

- **Lost swarm.pem**: Delete old file and run from scratch with same email
- **Multiple nodes**: Generate new `swarm.pem` for each, use same email
- **Moving machines**: Backup `swarm.pem` and transfer to new machine

---

## Logs and Monitoring

### Log Files Location

```
/logs/
├── yarn.log              # Modal login server logs
├── swarm.log             # Main RL Swarm application logs
└── wandb/                # Training run logs (if using Weights & Biases)
    └── debug.log
```

### Check Logs

```bash
# View main swarm logs
tail -f logs/swarm.log

# View login server logs
tail -f logs/yarn.log

# View prediction market logs
cat logs/prg_record.txt
```

---

## Common Issues & Solutions

### Peer Skipped a Round
- **Cause**: Your device is slower than the swarm pace
- **Effect**: Training jumps rounds (e.g., 100 → 102, skipping 101)
- **Solution**: Normal behavior, no action needed

### Model Not Training
- **Consumer devices**: Wait 20+ minutes (slower hardware)
- **Check logs**: Review `swarm.log` for errors

### Login Issues After Previous Session
```bash
# Click "Logout" on login screen
# Delete swarm.pem file
sudo rm swarm.pem
```

### VM/VPS Port Forwarding
```bash
# SSH with port forwarding
ssh -L 3000:localhost:3000 user@your-server
```

For cloud providers (e.g., GCP):
```bash
gcloud compute ssh --zone "us-central1-a" [your-vm] --project [your-project] -- -L 3000:localhost:3000
```

### OOM Errors (Out of Memory)
```bash
# MacBook fix
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
```

### Broken Pipe on VM
1. Press `Ctrl+C` to kill all processes
2. Restart the script: `bash run_rl_swarm.sh`

---

## Advanced Configuration

### Using Screen for Persistent Sessions

```bash
# Start a screen session
screen -S rl-swarm

# Run the application
source .venv/bin/activate && bash run_rl_swarm.sh

# Detach from screen: Ctrl+A then D

# Reattach to session
screen -r rl-swarm
```

### Custom Model Selection

During setup, you can specify a custom Hugging Face model:
- Paste the repo/model name when prompted
- Ensure your hardware can handle the model size

### Experimental Mode

For advanced users who want to experiment with GenRL library:
- Edit configuration: `rgym_exp/config/rg-swarm.yaml`
- Refer to [GenRL Getting Started Guide](https://github.com/gensyn-ai/genrl/blob/main/getting_started.ipynb)

---

## Hardware Recommendations

### Large Model Pool (Powerful GPUs)
- Assigned models: 1.5B - 1.7B parameters
- Recommended: RTX 4090, A100, H100

### Small Model Pool (Consumer Hardware)
- Assigned models: 0.5B - 0.6B parameters
- Recommended: RTX 3090 or CPU with 32GB+ RAM

**Note**: Model assignment is automatic based on detected hardware to maximize swarm utility.

---

## Support & Community

- **Issues**: [GitHub Issues](https://github.com/gensyn-ai/rl-swarm/issues)
- **Discord**: [Join Gensyn Community](https://discord.gg/AdnyWNzXh5)
- **Blog**: [blog.gensyn.ai](https://blog.gensyn.ai/)
- **Dashboard**: [dashboard.gensyn.ai](https://dashboard.gensyn.ai/)

---

## Quick Reference

```bash
# Check if Tailscale is running
tailscale status

# View Tailscale IP
tailscale ip -4

# Restart RL Swarm
source .venv/bin/activate && bash run_rl_swarm.sh

# Stop all processes
# Press Ctrl+C in terminal

# Check Python version
python3 --version

# View GPU info (NVIDIA)
nvidia-smi
```

---

**Note**: This is experimental software provided as-is for early adopters of the Gensyn Protocol. Report issues on GitHub with logs, device info, and OS details.
