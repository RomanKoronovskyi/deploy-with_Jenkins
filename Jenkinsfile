pipeline {
  agent {
    docker {
      image 'docker:24.0.6-dind'
      args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    DEPLOY_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    CONTAINER_NAME = "app-${env.BRANCH_NAME}-container"
  }

  stages {
    stage('Build Image') {
      steps {
        sh "docker --version"
        sh "docker build -t app-image:${env.BRANCH_NAME} ."
      }
    }

    stage ('Deploy') {
      steps {
        sh "docker stop ${CONTAINER_NAME} || true"
        sh "docker rm ${CONTAINER_NAME} || true"
        sh "docker run -d --name ${CONTAINER_NAME} -p ${DEPLOY_PORT}:3000 app-image:${env.BRANCH_NAME}"
      }
    }
  }
}

