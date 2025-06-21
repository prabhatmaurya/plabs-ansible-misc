# Docker CE Role

This Ansible role installs and configures Docker CE (Community Edition) on CentOS/RHEL and Ubuntu systems.

## Requirements

- Ansible 2.9 or higher
- CentOS/RHEL 7 or 8, or Ubuntu 20.04/22.04/24.04
- Root or sudo privileges

## Role Variables

### Default Variables

```yaml
# Remove old Docker versions before installation
docker_remove_old_versions: true

# Docker GPG key URLs (OS-specific)
docker_gpg_key_url_centos: https://download.docker.com/linux/centos/gpg
docker_gpg_key_url_ubuntu: https://download.docker.com/linux/ubuntu/gpg

# Docker repository URLs (OS-specific)
docker_repository_url_centos: https://download.docker.com/linux/centos/{{ ansible_distribution_major_version }}/x86_64/stable
docker_repository_url_ubuntu: https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} stable

# Docker daemon configuration (optional)
docker_daemon_config:
  log-driver: "json-file"
  log-opts:
    max-size: "10m"
    max-file: "3"
  storage-driver: "overlay2"
  storage-opts:
    - "overlay2.override_kernel_check=true"

# Users to add to docker group (optional)
docker_users: []
```

### Variable Files

The role uses the following OS-specific package lists defined in `vars/main.yml`:

**CentOS/RHEL:**
- `docker_old_packages_centos`: Packages to remove (old Docker versions)
- `docker_prerequisites_centos`: Required packages for Docker installation
- `docker_packages_centos`: Docker CE packages to install

**Ubuntu:**
- `docker_old_packages_ubuntu`: Packages to remove (old Docker versions)
- `docker_prerequisites_ubuntu`: Required packages for Docker installation
- `docker_packages_ubuntu`: Docker CE packages to install

## Dependencies

None.

## Example Playbook

### Basic Installation

```yaml
- hosts: servers
  roles:
    - docker-ce
```

### With Custom Configuration

```yaml
- hosts: servers
  vars:
    docker_daemon_config:
      log-driver: "json-file"
      log-opts:
        max-size: "20m"
        max-file: "5"
      storage-driver: "overlay2"
      insecure-registries:
        - "registry.example.com:5000"
    docker_users:
      - ansible
      - developer
  roles:
    - docker-ce
```

### Disable Old Version Removal

```yaml
- hosts: servers
  vars:
    docker_remove_old_versions: false
  roles:
    - docker-ce
```

## What the Role Does

1. **Prerequisites**: 
   - Removes old Docker versions (OS-specific)
   - Installs required packages (OS-specific)
   - Creates docker group
2. **Repository**: 
   - Adds Docker CE repository and GPG key (OS-specific)
3. **Installation**: 
   - Installs Docker CE packages (OS-specific)
4. **Configuration**: 
   - Creates Docker daemon configuration directory
   - Configures Docker daemon (if specified)
   - Adds users to docker group (if specified)
   - Verifies installation

## Supported Operating Systems

- **CentOS/RHEL 7/8**: Uses yum package manager
- **Ubuntu 20.04/22.04/24.04**: Uses apt package manager

## Handlers

- `restart docker`: Restarts the Docker service when configuration changes

## Testing

The role includes a test directory with example inventory and playbook:

```bash
cd roles/docker-ce/tests
ansible-playbook -i inventory test.yml
```

## License

MIT

## Author Information

PLabs 