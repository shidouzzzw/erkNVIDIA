# erkNVIDIA - NVIDIA Driver Installer for Debian-based Linux

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Platform: Debian/Ubuntu/Mint/Pop/Kali](https://img.shields.io/badge/Platform-Debian%20|%20Ubuntu%20|%20Mint%20|%20Pop%20|%20Kali-blue)](#supported-distributions)

`erkNVIDIA` is a **smart, interactive NVIDIA driver installer** for Debian-based Linux distributions. It automates the entire process — from GPU detection to driver installation, Secure Boot handling, and post-installation configuration.

## Why erkNVIDIA?

Installing NVIDIA drivers on Linux can be a headache:
- Which driver version do I need?
- How do I enable non-free repositories?
- What about Secure Boot?
- Why is my screen stuck at 800×600 after install?

`erkNVIDIA` solves all of this. One command. Done.

## Features

- **Automatic GPU Detection** — Identifies your exact NVIDIA card
- **Smart Driver Selection** — Picks the right driver for your GPU generation
- **Multiple Options** — Latest, open kernel module, legacy, CUDA-only, or full removal
- **Non-free Repository Setup** — Auto-configures repos for Debian, Ubuntu, Kali
- **Nouveau Blacklisting** — Safely disables the open-source nouveau driver
- **Secure Boot Support** — MOK key enrollment for signed kernel modules
- **CUDA Toolkit** — Optional CUDA installation
- **OpenCL Support** — Optional OpenCL installation
- **DKMS** — Automatic kernel module rebuilding on kernel updates
- **Wayland Support** — DRM modeset enabled by default
- **Persistence Daemon** — Better power management
- **Rollback** — Clean removal option
- **Dry-Run Mode** — See what would happen without making changes

## Supported Distributions

| Distribution | Status |
|-------------|--------|
| Debian (Bookworm+) | ✅ Full support |
| Ubuntu (20.04+) | ✅ Full support |
| Linux Mint | ✅ Full support |
| Pop!_OS | ✅ Full support |
| Kali Linux | ✅ Full support |
| elementary OS | ✅ Supported |
| Zorin OS | ✅ Supported |
| Other Debian-based | ⚠️ Experimental |

## Installation

```bash
# Clone the repository
git clone https://github.com/erk/erkNVIDIA.git
cd erkNVIDIA

# Make executable
chmod +x erkNVIDIA

# Run (interactive mode)
sudo ./erkNVIDIA
```

## Usage

### Interactive (recommended)

```bash
sudo ./erkNVIDIA
```

The script will:
1. Detect your distribution and GPU
2. Show available drivers
3. Ask what you want to do
4. Handle everything automatically

### Non-interactive

```bash
# Auto-detect and install best driver
sudo ./erkNVIDIA -y

# Install with CUDA toolkit
sudo ./erkNVIDIA -y --cuda

# Use open kernel module (Turing+ GPUs)
sudo ./erkNVIDIA --open

# Install specific driver version
sudo ./erkNVIDIA --driver 570
```

### Maintenance

```bash
# Remove all NVIDIA drivers
sudo ./erkNVIDIA --remove

# Dry run (see what would happen)
sudo ./erkNVIDIA -d
```

## What It Does

| Step | Description |
|------|-------------|
| 🔍 Detection | Identifies your OS, kernel, GPU model, and Secure Boot status |
| 📦 Repositories | Enables non-free/non-free-firmware (Debian) or Graphics Drivers PPA (Ubuntu) |
| 🚫 Nouveau | Blacklists the open-source nouveau driver to prevent conflicts |
| 🎯 Driver | Installs the selected NVIDIA driver with DKMS |
| 🔐 Secure Boot | Offers MOK key enrollment if Secure Boot is enabled |
| ⚙️ Configuration | Enables DRM modeset (Wayland), persistence daemon |
| ✅ Verification | Confirms driver is working via nvidia-smi |
| 🔄 Cleanup | Removes old packages, updates boot configuration |

## Post-Installation

1. **Reboot** — `sudo reboot`
2. **Verify** — `nvidia-smi`
3. **Optimus laptops** — `sudo prime-select nvidia`
4. **Wayland** — Enabled automatically via DRM modeset

## Logging

All operations are logged to `/tmp/erkNVIDIA.log`. Check this file if something goes wrong.

## Requirements

- **Root access** (sudo)
- **Internet connection** (for downloading packages)
- **NVIDIA GPU** (obviously)

Dependencies (`lspci`, `awk`, `lsb_release`, etc.) are installed automatically if missing.

## Uninstall

```bash
sudo ./erkNVIDIA --remove
```

Or manually:

```bash
sudo apt-get remove --purge '*nvidia*'
sudo apt-get autoremove --purge
sudo rm -f /etc/modprobe.d/nvidia-blacklist-nouveau.conf
sudo update-initramfs -u
```

## Troubleshooting

### "No NVIDIA GPU detected"

Make sure your NVIDIA card is properly seated and powered. Check with `lspci | grep -i nvidia`.

### "Cannot detect distribution"

Ensure `lsb_release` is installed or that `/etc/os-release` exists.

### "Driver not loaded after reboot"

Check Secure Boot — if enabled, you may need to sign the module. Run `mokutil --sb-state` to check.

### "Screen resolution stuck at 800×600"

This is normal until the NVIDIA driver is loaded. Run `nvidia-smi` to verify, then adjust resolution in Display Settings.

### "DKMS build failed"

Check `dmesg | tail -20` or `/var/log/dkms.log` for details. Make sure `linux-headers-$(uname -r)` is installed.

## Contributing

Pull requests are welcome! For major changes, please open an issue first.

## License

[MIT](LICENSE) © erk
