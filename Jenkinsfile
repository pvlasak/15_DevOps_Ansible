pipeline {
    agent any
    tools {
        maven 'Maven3.9'
    }
    environment {
        ANSIBLE_SERVER = "165.22.90.213"
    }
    stages {
        stage("Copy files") {
            steps {
                script {
                    echo "copying files to ansible server..."
                    sshagent(credentials: ['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/root/"
 
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ansible', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp ${keyfile} root@${ANSIBLE_SERVER}:/root/.ssh/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage ("execute ansible playbook") {
            steps {
                script {
                    echo "starting ansible playbook to configure ec2 instances"
                    def remote = [:]
                    remote.name = 'ansible-server'
                    remote.host = "${ANSIBLE_SERVER}"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identityFile = keyfile
                        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                            sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml -e dockerhub_password=${DOCKER_PASS}"                           
                        }
                    }
                }
            }            
        }
    }
}