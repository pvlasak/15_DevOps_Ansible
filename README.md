**branch jenkins_ansible**
- *Prerequisities:*
    1. Ansible server is running as Digital Ocean Droplet
    2. Ansible server has installed python3, python3-boto3 and python3-botocore
    3. AWS credentials are available in the file `~/.aws/credentials`
    4. ansible is installed on the ansible server
    5. EC2 instance is running on AWS and is accessible with a private key
    
#### Jenkinsfile is divided into two stages:
A. Copy all files needed for Ansible configuration - ansible playbook, docker compose file, vars file, ansible.cfg file
- Jenkins `sshagent plugin` is used to copy ansible files to ansible server
- AWS private key saved in Jenkins as credential `SSH Username with private key`
- Jenkins accesses the AWS private key and copies a file to `/root/.ssh/` on ansible server

B. execution of ansible commands on remote ansible server 

- Jenkins plugin **SSH Pipeline Steps** must be installed, example is to be found in the plugin documentation
- remote server must be defined as a groovy object.
- using plugins `withCredentials` and `sshCommand` Jenkins is able to execute ansible playbook on a remote ansible server and configure AWS EC2 instances by running a docker compose file.
