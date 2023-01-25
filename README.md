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
## Lynis 3.0.8 results

<img width="620" alt="image" src="https://user-images.githubusercontent.com/54890287/214538043-221712e3-22ea-4975-85fc-5b231180ed21.png">

## Todo:

Add role for configure AIDE
Add cron job for scheduled scanning using Lynis
Configure password hashing rounds in /etc/login.defs
Configure minimum password age in /etc/login.defs
Disable drivers like USB storage when not used
Enable logging to an external logging host for archiving purposes and additional protection
When possible set expire dates for all password protected accounts

Review and apply recommended values:

```
[+] Kernel Hardening
------------------------------------
    - Comparing sysctl key pairs with scan profile
    - dev.tty.ldisc_autoload (exp: 0)                         [ DIFFERENT ]
    - fs.protected_fifos (exp: 2)                             [ DIFFERENT ]
    - fs.suid_dumpable (exp: 0)                               [ DIFFERENT ]
    - kernel.kptr_restrict (exp: 2)                           [ DIFFERENT ]
    - kernel.modules_disabled (exp: 1)                        [ DIFFERENT ]
    - kernel.perf_event_paranoid (exp: 3)                     [ DIFFERENT ]
    - kernel.sysrq (exp: 0)                                   [ DIFFERENT ]
    - kernel.unprivileged_bpf_disabled (exp: 1)               [ DIFFERENT ]
    - net.core.bpf_jit_harden (exp: 2)                        [ DIFFERENT ]
    - net.ipv4.conf.all.log_martians (exp: 1)                 [ DIFFERENT ]
    - net.ipv4.conf.all.rp_filter (exp: 1)                    [ DIFFERENT ]
    - net.ipv4.conf.all.send_redirects (exp: 0)               [ DIFFERENT ]
    - net.ipv4.conf.default.log_martians (exp: 1)             [ DIFFERENT ]
```
