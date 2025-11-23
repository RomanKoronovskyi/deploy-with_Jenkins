pipeline {
    agent any

    environment {
      BUILD_PORT="${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
      CONTAINER_NAME="container-${env.BRANCH_NAME}"
    }

    stages {
        stage('checkout') {
          steps {
            cleanWs()
            checkout scm
          }
        }

        stage('Build') {
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
    }
}
