**branch dyn_inventory_ec2**
- ansible plugin `amazon.aws.aws_ec2` is used to get inventory hosts from AWS EC2
- plugin needs python, boto3 and botocore libraries to be installed. 
- dynamic invetory filename must end with the string **aws_ec2.yaml**.
- plugin aws_ec2 must be enabled in `ansible.cfg` file like *enable_plugin = aws_ec2*
- `ansible.cfg` file has to enable the plugin : *enable_plugins = amazon.aws.aws_ec2*
- execute inventory plugin and list inventory output: *ansible-inventory -i dyn_inventory_aws_ec2.yaml --list* OR *ansible-inventory -i dyn_inventory_aws_ec2.yaml --graph*
- instead of hosts file we can configure ansible to tak the dynamic inventory and run playbook as: *ansible-playbook -i dyn_inventory_aws_ec2.yaml deploy_docker.yaml*
- dynamic inventory file may also filter instances, for example according to tag name - *filters: tag:Name: dev**
- dynamic inventory file may also group instances according different attributes - e.g. tag, instance_type and use those groups in a playbook to target only some of the instances. 
