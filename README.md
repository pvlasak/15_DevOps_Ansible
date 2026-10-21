# 15_DevOps_Ansible
branch containing ansible inventory file and ansible playbook to deploy node js application on remote server
ansible playbook uses variable defintions inside app_deploy_vars and is thefore parameterized.
ansible playbook contains these plays:
update apt repository and installs node and npm
creates new linux user
deploys node js application a) copies and unpacks tar archive b) installs dependencies using package.json c) starts server.js d) checks if node process is running under defined user.
