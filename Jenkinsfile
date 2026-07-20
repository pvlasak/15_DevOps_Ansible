pipeline {
    agent any
    tools {
        maven 'Maven3.9'
    }
    environment {
        ANSIBLE_SERVER = "207.154.224.174"
    }
    stages {
        stage("Copy files") {
            steps {
                script {
                    echo "copying files to ansible server..."
                    sshagent(credentials: ['ansible-user']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/root/"
 
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ansible', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh "scp ${keyfile} root@${ANSIBLE_SERVER}:/root/ssh-key.pem"
                    }
                }
            }
        }
    }
}