**branch nexus_deploy**
- `deploy_nexus.yaml` - definition of ansible playbook
- updates apt cache, installs openjdk and net-tools
- downloads tar archive, unpacks it and finds the nexus directory
- renames nexus directory to `nexus` only in case the task was not initiated before. For this check the builtin.stat module is used to get facts about the file. Rename task evaluates the `when conditional` to check if it should be skipped or executed. 
- creates group, user called *nexus* and changes the ownership of nexus application files.
- edit binaries through `lineinfile` module and starts the application by `command` module
- checks if the nexus process is running by printing a process status. 
- port where the nexus application is listening is checked by netstat tool. 
