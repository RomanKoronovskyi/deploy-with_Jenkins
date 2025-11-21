pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Checkout') {
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
                dir("${env.WORKSPACE}") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Archive') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh 'rm -rf *.tar.gz'
                    sh 'tar -czf build_artifact.tar.gz public scripts src Dockerfile README.md package.json'
                    archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
                }
            }
        }
    }
}

