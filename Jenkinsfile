pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps{
                script {
                    echo "copying all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* ec2-user@18.212.143.142:/home/ec2-user"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'scp $keyfile ec2-user@18.212.143.142:/home/ec2-user/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage("configure ec2 instances with ansible") {
            steps {
                script {
                    echo "calling ansible playbook"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "18.212.143.142"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                             remote.user = user
                             remote.identityFile = keyfile
                             sshCommand remote: remote, command: "ls -l"
                    }

                }
            }
        }
    }
}