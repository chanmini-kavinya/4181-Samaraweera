pipeline {
    agent any 
   
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/chanmini-kavinya/4181-Samaraweera'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t chanmini/nodeapp:%BUILD_NUMBER% .'
            }
        }
        stage('Run Docker Container') {
            steps {
                bat 'docker run -d -p 9000:3000 chanmini/nodeapp:%BUILD_NUMBER%'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'password-4181', variable: 'password')]) {
                    bat "docker login -u chanmini -p ${password}"
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push chanmini/nodeapp:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}