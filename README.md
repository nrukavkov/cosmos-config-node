# Ansible Cosmos Node

An Ansible toolkit for secure host and installing cosmos node. This toolkit based on https://github.com/hyphacoop/cosmos-ansible.

## Requirements 

- Python 3
- Ansible 

## Playbooks

### init.yml

Install and configure new server. Init.yaml contains roles like:

- `ssh-secure` - main security module installs and configure system tools for imoroving secutity. It changes default 22 port to 22222, installing fail2ban, mailware detection tool, acct and monitoring tools. You can change default ssh port to another in `inventory/hosts.yaml`.
- `ssh-permissions` - add new users to server. 

### node.yml

Install and configure Cosmos Node with node exporter monitoring

- `node` - install and configure Cosmos Node.
- `node-exporter` - install node-exporter and expose port 9100 to get metrics. Example http://YOUR_HOST:9100/metrics
- `common` - main tools which are necessary for installing Cosmos Node

## Quick Start

1. Clone this repository
2. Set up SSH access to the target machine
3. Set you public key in `init.yml`
4. Open inventory/hosts.yaml and set your hostname/IP addreess:

    ```
    children:
      node:
        hosts:
          "65.109.164.134":
    ```

5. Run the init playbook. This playbook make a base secure settings for ssh, create user `ansible` with public key and will change ssh port to 22222.
    ```
    ansible-playbook init.yml -e 'ansible_user=root' -e 'ansible_port=22' --ask-pass
    ```
6. Run node playbook for installing and configuing node. Additionally this playbook installs node-exporter with additional gaiad metrics.

    ```
    ansible-playbook node.yml
    ```
