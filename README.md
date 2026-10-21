**branch deploy_ec2_with_roles**
- ansible playbook has new roles *create_user* and *start_containers*
- each role is defined by a given sructure of data and few types of data are existing there:
    1. `roles/<role_name>/tasks/main.yaml` defining role's tasks to be executed. 
    2. `roles/<role_name>/defaults/main.yaml` defining default values of ansible variables
    3. `roles/<role_name>/vars/main.yaml` - assigning values to variables used for a role. 
    4. `roles/<role_name>/files/` - static files that are referenced by a role. 
