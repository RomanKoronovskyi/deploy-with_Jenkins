pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                    args '-u root:root'
                }
            }
            steps {
                checkout scm
                sh 'ls -la'
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
    }
}

