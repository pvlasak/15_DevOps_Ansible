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
                            sh 'scp ${keyfile} root@${ANSIBLE_SERVER}:/root/ssh-key.pem'
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

                    withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ansible', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identity = keyfile
                        sshCommand remote: remote, command: 'ls -l'
                    }
                }
            }            
        }
    }
}