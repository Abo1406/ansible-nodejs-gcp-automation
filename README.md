# Ansible Node.js Deployment on GCP

[![Ansible](https://img.shields.io/badge/Ansible-2.10+-red?logo=ansible)](https://www.ansible.com/)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-brightgreen?logo=node.js)](https://nodejs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Automated infrastructure provisioning and deployment pipeline for Node.js applications on Google Cloud Platform. This Ansible playbook handles everything from VM creation to application deployment and process management.

## 🚀 Key Features
- **GCP Instance Provisioning**: Creates optimized compute instances
- **Environment Bootstrapping**: Installs Node.js, npm, and dependencies
- **Secure Deployment**: Dedicated application user with restricted privileges
- **Process Management**: Background service deployment with status verification
- **Idempotent Operations**: Safe for repeated execution

## ⚙️ Technologies Used
| Component       | Implementation |
|-----------------|----------------|
| **Cloud**       | Google Cloud Platform (GCP) |
| **Automation**  | Ansible 2.10+ |
| **Runtime**     | Node.js 18.x |
| **OS**          | Rocky Linux 9 / RHEL-compatible |
| **Packaging**   | TGZ archive |

## 📂 Repository Structure
```bash
ansible-nodejs-gcp-automation/
├── inventory             # Target host configuration
├── ansible.cfg           # Ansible environment settings
├── deploy-nodejsapp.yml  # Main deployment playbook
├── nodejs-app-1.0.0.tgz  # Sample Node.js application bundle
└── README.md             # This documentation
```
## 🛠️ Prerequisites
- Google Cloud Platform account with billing enabled
- Ansible control machine (Linux/macOS)
- Installed dependencies:
```bash
pip install ansible google-auth
gcloud auth application-default login
```
## 🚀 Execution
Deploy with a single command:
```bash
ansible-playbook -i inventory deploy-nodejsapp.yml
```
## 📝 Playbook Breakdown
### Core Workflow:
1. System Preparation:
- OS package updates
- Node.js + npm installation
- Support utilities (zip/gzip)

2. Application Deployment:
- Secure file transfer
- Archive extraction
- Dependency installation via npm

3. Service Management:
- Background application startup
- Process verification
- Status reporting
### Critical Tasks:
```bash
- name: Start Application (Background)
  command:
    cmd: node server.js
    chdir: /root/package/app/
  async: 1000
  poll: 0

- name: Verify Deployment
  shell: "ps aux | grep node"
  register: app_status
  changed_when: false

- name: Display Runtime Status
  debug:
    msg: "Application running with PID(s): {{ app_status.stdout }}"
```
## 🧪 Verification
Confirm application availability:
```bash
ansible node2 -m shell -a "curl -s localhost:3000"
```
## 📜 License
MIT License - See LICENSE for details.
