pipeline {
    agent any

    environment {
      BUILD_PORT="${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
      CONTAINER_NAME="container-${env.BRANCH_NAME}"
    }

    stages {
        stage('checkout') {
          steps {
            checkout scm
          }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Archive') {
            steps {
                sh 'rm -f build_artifact.tar.gz'
                sh 'tar -czf build_artifact.tar.gz public scripts src Dockerfile README.md package.json'
                archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            }
        }

        stage('Containerized') {
          agent{
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
          }
          steps {
            sh "docker stop ${env.CONTAINER_NAME} || true"
            sh "docker rm ${env.CONTAINER_NAME} || true"
            sh "docker build -t app-image:${env.BRANCH_NAME} ." 
            sh "docker run -d --name ${env.CONTAINER_NAME} -p ${env.DEPLOY_PORT}:3000 app-image:${env.BRANCH_NAME}"
          }
        }
    }
}
