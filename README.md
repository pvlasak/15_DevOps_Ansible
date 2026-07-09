# 15_DevOps_Ansible
repository to store project on configuration management with Ansible

## Ansible Commands
- *ansible all -i hosts -m ping* - interact with all servers in the inventory file named `hosts` and execute ansible module ping
- *ansible droplet -i hosts -m ping*  - target only the server in the group `droplet`
- *ansible 165.232.127.190 -i hosts -m ping* - target specific server identifed by IP Address
- *ansible-playbook -i hosts example_playbook.yaml* - start ansible playbook `hosts example_playbook.yaml` with inventory file named `hosts`

## Host Key Checking
- *ssh-keyscan -H IP_address >> ~/.ssh/known_hosts* - info about the server is added into `~/.ssh/known_hosts` file. 
- location where public access key is saved: `~/.ssh/authorized_keys`
- copy public ssh key into `~/.ssh/authorized_keys` on target server: *ssh-copy-id -i identity_file root@ip_address*
- disable host key checking in `ansible.cfg` file in [defaults] sections as: *host_key_checking = False*
