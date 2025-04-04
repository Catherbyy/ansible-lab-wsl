# 🛠️ Ansible + Docker Lab (WSL Edition)

This project sets up a lightweight Ansible lab using Docker containers running inside WSL (Windows Subsystem for Linux). It's perfect for practicing automation, configuration management, and infrastructure as code — all from a single machine.

---

## 🚀 Lab Architecture

- **Control Node**: WSL Ubuntu (Ansible installed here)
- **Managed Nodes**: Docker containers with SSH servers (Ubuntu base)

---

## ⚙️ Prerequisites

On Windows:

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- WSL 2 with Ubuntu installed (`wsl --install`)
- Enable WSL integration in Docker Desktop (⚙️ → Resources → WSL Integration → ✅ Ubuntu)

Inside Ubuntu (WSL):

```bash
sudo apt update
sudo apt install ansible sshpass

