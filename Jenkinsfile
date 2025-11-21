pipeline {
  agent any
  stages {
	tools {
      nodejs "Node18"
    }
    stage('Build') {
      steps {
	    sh "rm -rf *.tar.gz"
        sh "npm install"
	    sh "tar -czf archive-${BUILD_NUMBER}.tar.gz public scripts src Dockerfile package.json README.md"
      }
    }
  }
}
