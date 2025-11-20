pipeline {
    agent none 
    
    environment {
        DEPLOY_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        CONTAINER_NAME = "app-${env.BRANCH_NAME}-instance"
    }

    stages {
        stage('Checkout SCM') {
            agent any 
            steps {
                checkout scm
            }
        }
        
        stage('Build Image') {
            agent { 
                docker { 
                    image 'docker:latest' 
                    args '-v /var/run/docker.sock:/var/run/docker.sock' 
                }
            }
            steps {
                sh "docker build -t app-image:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            agent { 
                docker { 
                    image 'docker:latest' 
                    args '-v /var/run/docker.sock:/var/run/docker.sock' 
                }
            }
            steps {
                sh "docker stop ${env.CONTAINER_NAME} || true"
                sh "docker rm ${env.CONTAINER_NAME} || true"
                sh "docker run -d --name ${env.CONTAINER_NAME} -p ${env.DEPLOY_PORT}:3000 app-image:${env.BRANCH_NAME}"
            }
        }
    }
}
