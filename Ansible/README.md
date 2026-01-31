# Homelab Self-hosting Automation

This repository contains scripts, Ansible playbooks, and Docker configurations to automate the setup and management of a self-hosted homelab environment.

## 🏗 Project Structure

- **`Ansible/`**: Proxmox LXC automation and container configuration.
  - `playbooks/`: Create, configure, and manage LXC containers.
  - `inventories/`: Production inventory configuration.
  - `vars/`: Proxmox host-specific variables and encrypted secrets.
  - `roles/`: Custom Ansible roles.
- **`docker/`**: A collection of `docker-compose` files for various services (Adguard, Jellyfin, Immich etc.).

## 🚀 Getting Started with Ansible

### 1. Prerequisites
Install the required Ansible roles and collections:
```bash
cd Ansible
ansible-galaxy install -r playbooks/requirements.yml
```

### 2. Secrets Management
The project uses **Ansible Vault** for sensitive data (tokens, SSH keys).
- Create your own `Ansible/.vault_pass.txt` (ignored by git).
- Define secrets in `Ansible/vars/vault.yml`.

**Create a vault password file:**
```bash
echo "your-strong-password" > Ansible/.vault_pass.txt
chmod 600 Ansible/.vault_pass.txt
```

**Encrypt the vault file:**
```bash
cd Ansible
ansible-vault encrypt vars/vault.yml --vault-password-file .vault_pass.txt
```

**Decrypt the vault file (to edit):**
```bash
cd Ansible
ansible-vault decrypt vars/vault.yml --vault-password-file .vault_pass.txt
```

### 3. Setup and Configure LXC Container
Run the setup playbook to configure your LXC container (install Docker, Hawser, etc.):
```bash
ansible-playbook -i inventories/production.ini playbooks/lxc-setup.yml \
  -e env=pve-h4 \
  --vault-password-file .vault_pass.txt
```

### 4. Manage Container
Use `lxc-manage.yml` with tags to create, start, stop, or delete containers:

**Create a container:**
```bash
ansible-playbook -i inventories/production.ini playbooks/lxc-manage.yml \
  -e env=pve-h4 \
  -e id=123 \
  -e hostname=my-new-service \
  --tags create \
  --vault-password-file .vault_pass.txt
```

**Start a container:**
```bash
ansible-playbook -i inventories/production.ini playbooks/lxc-manage.yml \
  -e env=pve-h4 \
  -e id=123 \
  --tags start \
  --vault-password-file .vault_pass.txt
```

**Stop a container:**
```bash
ansible-playbook -i inventories/production.ini playbooks/lxc-manage.yml \
  -e env=pve-h4 \
  -e id=123 \
  --tags stop \
  --vault-password-file .vault_pass.txt
```

**Delete a container:**
```bash
ansible-playbook -i inventories/production.ini playbooks/lxc-manage.yml \
  -e env=pve-h4 \
  -e id=123 \
  --tags delete \
  --vault-password-file .vault_pass.txt
```

## 🛠 Management Tasks

- **Update Inventory**: Add new container details in `Ansible/inventories/production.ini` and host details in `Ansible/vars/*.yaml` files.

---
*Note: This repository is intended for personal homelab use. Go through the scripts carefully before using to your needs.*
