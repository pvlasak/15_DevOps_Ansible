**branch deploy_ec2**
- this branch shows how the infrastructure provisioning on AWS using terraform can be coupled with configuration file from ansible.
- Ansible-playbook is divided into four plays:
    1. ensures that the server SSH port 22 is open 
    2. makes the docker is running, adds user to the docker group to run docker commands without sudo
    3. installs docker compose, download executables, sets permissions
    4. runs images pulled from public and private docker repository by docker compose 

- Terraform may automatically starts an ansible configuration through *provisioner local-exec*
- provisioner can be wrapped into a terraform *null_resource* defintion and optional trigger parameter can be defined and may have instance's public ip address as a value. If ip address changes (ec2 instance is re-created), the null_resource gets executed. 
