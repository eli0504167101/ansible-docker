
---

# Ansible Docker Lab (Agentless)

A lightweight lab simulating a Master-Slave architecture using Docker containers.

## 🏗️ Architecture
* **Master (Control Node):** Ubuntu 22.04 with Ansible & OpenSSH client.
* **Slave (Managed Node):** Ubuntu 22.04 with OpenSSH server & Python3.
* **Networking:** Dedicated Docker network (`ansible-net`).

## 🚀 Quick Start

### 1. Setup Infrastructure
```bash
# Create network and build images
docker network create ansible-net
docker build -t ansible-master -f Dockerfile.master .
docker build -t ansible-slave -f Dockerfile.slave .

# Run containers
docker run -d --name slave-node --network ansible-net ansible-slave
docker run -it --name master-node --network ansible-net ansible-master bash
```

### 2. Configure Connection (Inside Master)
Access the Master container and set up passwordless SSH:
```bash
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id user@slave-node
```

### 3. Inventory & Test
Create `inventory.yaml` and run the connectivity test:
```yaml
all:
  hosts:
    slave-node:
      ansible_user: user
      ansible_connection: ssh
```

**Run Ping:**
```bash
ansible all -i inventory.yaml -m ping
```

## 🛠️ Key Concepts Demonstrated
* **Agentless Management:** Managing nodes via SSH/Python without local Ansible installation.
* **Infrastructure as Code:** Using Dockerfiles to standardize environments.
* **Linux Security:** Non-root user management and SSH key authentication.

---

