pipeline {
  agent {
    docker {
      image 'docker:27.0-cli'   // CLI here
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {
    stage('Build image') {
      steps {
        sh "docker --version"
        sh "docker build -t app-image:${env.BRANCH_NAME} ."
      }
    }

    stage('Deploy') {
      steps {
        sh "docker stop app-${env.BRANCH_NAME}-container || true"
        sh "docker rm app-${env.BRANCH_NAME}-container || true"
        sh "docker run -d --name app-${env.BRANCH_NAME}-container -p ${env.BRANCH_NAME == 'main' ? '3000' : '3001'}:3000 app-image:${env.BRANCH_NAME}"
      }
    }
  }
}

