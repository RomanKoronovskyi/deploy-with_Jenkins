pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker { image 'node:18' }
            }
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Archive') {
            steps {
                sh 'rm -rf *.tar.gz'
                sh 'tar -czf build_artifact.tar.gz .'
                archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            }
        }
    }
}

