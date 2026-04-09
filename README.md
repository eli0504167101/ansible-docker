
---

# Ansible Lab over Docker (Master-Slave Architecture)

This project demonstrates a local infrastructure for configuration management using **Ansible** and **Docker**. It simulates a real-world scenario where a Control Node (Master) manages a Remote Node (Slave) in an agentless manner.

## 🏗 Architecture Overview

The setup consists of two main components running in a dedicated Docker network:

1.  **Ansible Master (Control Node):**
    * **OS:** Ubuntu 22.04
    * **Tools:** Ansible, OpenSSH client, Python3.
    * **Role:** The "brain" of the operation. It stores the inventory and executes automation tasks.
2.  **Ansible Slave (Managed Node):**
    * **OS:** Ubuntu 22.04
    * **Tools:** OpenSSH server, Python3.
    * **Role:** A simulated remote server. It remains "agentless," meaning no Ansible software is installed on it—only a standard SSH daemon and Python interpreter.



---

## 🚀 Step-by-Step Implementation

### 1. Infrastructure Setup
* **Networking:** Created a dedicated Docker network (`ansible-net`) to allow container communication by name/IP.
* **Dockerization:** * Built a custom `Dockerfile.master` containing the Ansible engine.
    * Built a custom `Dockerfile.slave` configured with a running SSH service and a dedicated user (`user`).

### 2. Security & Connection
* **SSH Key Exchange:** Generated an RSA key pair on the Master node and transferred the public key to the Slave node to allow secure, passwordless authentication.
* **Identity Management:** Configured a non-root user on the Slave to follow security best practices.

### 3. Inventory Configuration
Created an `inventory.yaml` file on the Master node. This file acts as the "source of truth," mapping the Slave's IP address, SSH user, and connection type.

```yaml
all:
  hosts:
    slave-node:
      ansible_host: <SLAVE_IP>
      ansible_user: user
      ansible_connection: ssh
```

### 4. Verification (The "Ping" Test)
Executed the Ansible `ping` module to verify end-to-end connectivity:
```bash
ansible all -i inventory.yaml -m ping
```
**Result:** Successful "pong" response, confirming that the Master can execute Python code on the remote Slave via SSH.

---

## 🛠 Skills Demonstrated
* **Containerization:** Docker, Dockerfiles, Networking.
* **Automation:** Ansible Inventory, Modules (ping).
* **Linux Administration:** SSH configuration, User management, WSL workflow.
  

