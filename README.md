# ansible-lab-wsl
Tutorial: Ansible lab running inside Docker containers on WSL

# 🛠️ Ansible + Docker Lab (WSL Edition)

This project sets up a lightweight Ansible lab using Docker containers running inside WSL (Windows Subsystem for Linux). It's perfect for practicing automation, configuration management, and infrastructure as code — all from a single machine.

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

#Without sshpass, you may encounter an SSH auth error

Build the Docker Image
Create a file called dockerfile.ansible_target with this content:

FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y openssh-server sudo python3 && \
    mkdir /var/run/sshd && \
    echo 'root:<your-password-here>' | chpasswd && \
    sed -i 's/#\?PermitRootLogin .*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#\?PasswordAuthentication .*/PasswordAuthentication yes/' /etc/ssh/sshd_config

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

Replace <your-password-here> with any password you like for testing.
⚠️ Do not use sensitive or production passwords.

Then build the image:
docker build -f dockerfile.ansible_target -t ansible-target .

Run a Managed Node (Docker Container)
## 🐳 Run a Managed Node (Docker Container)

Run one container and expose port 2222 for SSH:

```bash
docker run -d --name node1 -p 127.0.0.1:2222:22 ansible-target

🔒 Security Note:
Binding to 127.0.0.1 ensures that port 2222 is only accessible from your local machine.
This prevents external access to the container and is safe for lab and testing environments.

You can now SSH into the container:
ssh root@localhost -p 2222
# Use the password you set in the Dockerfile


Ansible Inventory File:
Create inventory.ini:

[node_targets]
localhost ansible_host=127.0.0.1 ansible_port=2222 ansible_user=root ansible_password=<your-password-here> ansible_ssh_common_args='-o StrictHostKeyChecking=no'

Replace <your-password-here> with the same one you set earlier.


Ansible Ping Test
ansible node_targets -i inventory.ini -m ping

Expected output:

localhost | SUCCESS => {
  "changed": false,
  "ping": "pong"
}
