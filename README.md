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

**branch deploy_nodejs_app**
- branch containing ansible inventory file and ansible playbook to deploy node js application on remote server
- ansible playbook uses variable defintions inside `app_deploy_vars` and is thefore parameterized. 
- ansible playbook contains these plays:
1. update apt repository and installs node and npm
2. creates new linux user
3. deploys node js application 
    a) copies and unpacks tar archive
    b) installs dependencies using package.json
    c) starts server.js
    d) checks if node process is running under defined user. 

**branch deploy_nexus**
- `deploy_nexus.yaml` - definition of ansible playbook
- updates apt cache, installs openjdk and net-tools
- downloads tar archive, unpacks it and finds the nexus directory
- renames nexus directory to `nexus` only in case the task was not initiated before. For this check the builtin.stat module is used to get facts about the file. Rename task evaluates the `when conditional` to check if it should be skipped or executed. 
- creates group, user called *nexus* and changes the ownership of nexus application files.
- edit binaries through `lineinfile` module and starts the application by `command` module
- checks if the nexus process is running by printing a process status. 
- port where the nexus application is listening is checked by netstat tool. 

**branch deploy_ec2**
- this branch shows how the infrastructure provisioning on AWS using terraform can be coupled with configuration file from ansible.
- Ansible-playbook is divided into four plays:
    1. ensures that the server SSH port 22 is open 
    2. makes the docker is running, adds user to a docker to run docker commands without sudo
    3. installs docker compose, download executables, sets permissions
    4. runs images pulled from public and private docker repository by docker compose 

- Terraform may automatically starts an ansible configuration through *provisioner local-exec*
- provisioner can be wrapped into a terraform *null_resource* defintion and optional trigger parameter can be defined and may have instance's public ip address as a value. 

**branch dyn_inventory_ec2**
- ansible plugin `amazon.aws.aws_ec2` is used to get inventory hosts from AWS EC2
- plugin needs python, boto3 and botocore libraries to be installed. 
- dynamic invetory filename must end with the string **aws_ec2.yaml**.
- `ansible.cfg` file has to enable the plugin : *enable_plugins = amazon.aws.aws_ec2* 
- dynamic inventory file may also filter instances, for example according to tag name
- dynamic inventory file may also group instances according different attributesme

**branch deploy_in_k8s**
- ansible `kubernetes.core.k8s` module is used to make configuration on Kubernetes cluster
- ansible `kubernetes.core.k8s` needs python and libraries such as pyyaml, kubernetes and jsonpatch.
- to access the kubernetes cluster a kubeconfig file is referenced in the ansible yaml file. 
- kubeconfig file can be downloaded from cluster to local through this command: <br>
*aws eks update-kubeconfig --region eu-central-1 --name myapp-eks-cluster --kubeconfig ~/terraform/kubeconfig_myapp-eks-cluster*
- kubeconfig can be also accessed through environmental variable *K8S_AUTH_KUBECONFIG*
- ansible configuration commands are executed from localhost.
