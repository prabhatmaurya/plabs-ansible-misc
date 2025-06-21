# Docker CE Installation on Ubuntu

This example demonstrates how to install Docker CE specifically on Ubuntu systems (20.04, 22.04, 24.04) using the `docker-ce` role.

## Quick Start for Ubuntu

1. **Update the inventory file** with your Ubuntu servers:
   ```bash
   # Edit inventory file
   vim inventory
   ```

2. **Run the playbook**:
   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

## Ubuntu-Specific Features

This playbook includes Ubuntu-specific configurations:

### Package Management
- Uses `apt` package manager instead of `yum`
- Automatically updates package cache
- Removes Ubuntu-specific old Docker packages

### Repository Management
- Adds Docker's official Ubuntu repository
- Uses Ubuntu-specific GPG key
- Configures repository for the correct Ubuntu release

### Docker Configuration
- Ubuntu-optimized daemon settings
- `live-restore: true` for better container management
- `userland-proxy: false` for better performance

### User Management
- Adds Ubuntu users to docker group
- Includes common Ubuntu usernames (`ubuntu`, `ansible`)

## Ubuntu 24.04 Specific Notes

For Ubuntu 24.04 (Noble Numbat):
- Uses the latest Docker CE repository
- Compatible with systemd and containerd
- Supports all modern Docker features

## Example Inventory for Ubuntu 24.04

```ini
[ubuntu_servers]
ubuntu24-01 ansible_host=192.168.1.10 ansible_user=ubuntu
ubuntu24-02 ansible_host=192.168.1.11 ansible_user=ubuntu
```

## Verification Commands

After installation, you can verify Docker on Ubuntu:

```bash
# Check Docker version
docker --version

# Test Docker functionality
docker run hello-world

# Check Docker service status
sudo systemctl status docker

# Verify user is in docker group
groups $USER
```

## Troubleshooting Ubuntu Installation

### Common Issues

1. **Permission denied errors**:
   ```bash
   # Add user to docker group and restart session
   sudo usermod -aG docker $USER
   newgrp docker
   ```

2. **Repository issues**:
   ```bash
   # Update package lists
   sudo apt update
   ```

3. **Service not starting**:
   ```bash
   # Check Docker service
   sudo systemctl status docker
   sudo journalctl -u docker
   ```

## Requirements

- Ubuntu 20.04, 22.04, or 24.04
- Ansible 2.9 or higher
- SSH access with sudo privileges 