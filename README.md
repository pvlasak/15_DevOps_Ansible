**branch deploy_in_k8s**
- ansible `kubernetes.core.k8s` module is used to make configuration on Kubernetes cluster
- ansible `kubernetes.core.k8s` needs python and libraries such as pyyaml, kubernetes and jsonpatch.
- to access the kubernetes cluster a kubeconfig file is referenced in the ansible yaml file. 
- kubeconfig file can be downloaded from cluster to local through this command: <br>
*aws eks update-kubeconfig --region eu-central-1 --name myapp-eks-cluster --kubeconfig ~/terraform/kubeconfig_myapp-eks-cluster*
- kubeconfig can be also accessed through environmental variable *K8S_AUTH_KUBECONFIG*
- ansible configuration commands are executed from localhost.
