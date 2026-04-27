# 🐧 Windows Subsystem for Linux (WSL) Guide

![Windows](https://img.shields.io/badge/Platform-Windows-blue)
![WSL](https://img.shields.io/badge/WSL-Enabled-green)
![Linux](https://img.shields.io/badge/Linux-Debian%20%7C%20Ubuntu-yellow)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

A simple guide to installing and running a Linux environment on Windows using **WSL**, including optional desktop environments and Remote Desktop access.

---

## 📚 Table of Contents

* [What is WSL?](#-what-is-wsl)
* [Install a Linux Distro](#-install-a-linux-distro)
* [Install a Desktop Environment](#-install-a-desktop-environment)
* [Access Desktop via RDP](#-access-desktop-via-rdp)
* [Tips](#-tips)

---

## 🧠 What is WSL?

**Windows Subsystem for Linux (WSL)** allows you to run a full GNU/Linux environment directly on Windows—without a virtual machine or dual boot.

You get:

* Native Linux terminal
* Package managers like `apt`
* Developer tools (Git, Python, Node, etc.)

---

## ⚙️ Install a Linux Distro

Open **PowerShell** or **Command Prompt** as Administrator:

```bash
wsl --install
```

List available distributions:

```bash
wsl --list --online
```

---

## 🖥️ Install a Desktop Environment

WSL is terminal-based by default, but you can install a GUI (XFCE, GNOME, KDE).

### 1. Update packages

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Tasksel

```bash
sudo apt install tasksel
```

### 3. Launch Tasksel

```bash
sudo tasksel
```

**Controls:**

* `Space` → Select/Deselect
* `Tab` → Navigate
* `Enter` → Confirm

---

## 🌐 Access Desktop via RDP

### 1. Install xrdp

```bash
sudo apt install xrdp
sudo systemctl enable xrdp
sudo systemctl start xrdp
```

---

### 2. Find your WSL IP

```bash
ip -br a
```

Look for:

```
eth0    UP    172.x.x.x
```

---

### 3. Connect from Windows

* Open **Remote Desktop Connection**
* Enter your WSL IP address
* Login with your Linux username & password

---

## 💡 Tips

* XFCE is lightweight and works best in WSL
* GNOME/KDE may feel slower without GPU acceleration
* Restart WSL if networking breaks:

```bash
wsl --shutdown
```

---

## 📄 License

This project is licensed under the MIT License.

---

## 🙌 Contributions

Feel free to fork this repo and improve the guide!
