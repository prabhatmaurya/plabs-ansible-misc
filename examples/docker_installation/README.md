# Docker CE Installation Example

This example demonstrates how to use the `docker-ce` role to install Docker CE on your servers.

## Quick Start

1. **Update the inventory file** with your target servers:
   ```bash
   # Edit inventory file
   vim inventory
   ```

2. **Run the playbook**:
   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

## Supported Operating Systems

This role supports:
- **CentOS/RHEL 7/8**: Uses yum package manager
- **Ubuntu 20.04/22.04/24.04**: Uses apt package manager

## Customization

### Basic Installation
For a basic Docker CE installation, you can use the playbook as-is. The role will automatically detect your OS and use the appropriate package manager.

### Custom Configuration
To customize Docker settings, modify the `docker_daemon_config` variable in the playbook:

```yaml
docker_daemon_config:
  log-driver: "json-file"
  log-opts:
    max-size: "20m"
    max-file: "5"
  storage-driver: "overlay2"
  insecure-registries:
    - "registry.example.com:5000"
```

### Add Users to Docker Group
To allow users to run Docker commands without sudo, add them to the `docker_users` list:

```yaml
docker_users:
  - ansible
  - developer
  - your_username
```

### Disable Old Version Removal
If you don't want to remove existing Docker installations:

```yaml
docker_remove_old_versions: false
```

## OS-Specific Behavior

The role automatically handles OS differences:

- **CentOS/RHEL**: Uses `yum` and `rpm_key` for package management
- **Ubuntu**: Uses `apt` and `apt_key` for package management
- **Repository URLs**: Automatically selects the correct Docker repository for your OS
- **Package Lists**: Uses OS-specific package names and dependencies

## Verification

The playbook includes a post-task that runs `docker run hello-world` to verify the installation.

## Requirements

- Ansible 2.9 or higher
- CentOS/RHEL 7/8 or Ubuntu 20.04/22.04/24.04 on target servers
- Root or sudo privileges on target servers 