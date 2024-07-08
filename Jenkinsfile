pipeline {
    agent any

    stages {
        stage('Install the dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Build the code') {
            steps {
                sh "npm run build"
            }
        }

        stage('Deploy') {
            steps {
                 withCredentials([sshUserPrivateKey(credentialsId: 'counter-app', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                     sh '''
                       mkdir -p ~/.ssh
                       ssh-keyscan -H 16.171.168.63 >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY -r build/* $SSH_USER@16.171.168.63:/var/www/html
                        ssh -i $SSH_KEY $SSH_USER@16.171.168.63 'sudo systemctl restart httpd'
                     '''
                 }   
            }
        }
    }
}
