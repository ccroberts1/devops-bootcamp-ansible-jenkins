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
                            sh "scp ${keyfile} ec2-user@18.212.143.142:/home/ec2-user/ssh-key.pem"
                        }
                    }
                }
            }
        }
    }
}