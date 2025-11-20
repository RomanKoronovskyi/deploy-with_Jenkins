pipeline {
  agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

  environment {
    DEPLOY_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    CONTAINER_NAME = "app-${env.BRANCH_NAME}-container"
  }

  stages {
    stage('Build image') {
      steps {
        echo "Building image for '${env.BRANCH_NAME}'"
        sh "docker build -t app-image:${env.BRANCH_NAME} ." 
      }
    }

    stage ('Deploy') {
      steps {
        echo "Deploy"

        sh "docker stop ${env.CONTAINER_NAME} || true"
        sh "docker rm ${env.CONTAINER_NAME} || true"

        sh "docker run -d --name ${env.CONTAINER_NAME} -p ${DEPLOY_PORT}:3000 app-image:${env.BRANCH_NAME}"
        echo "Deploy success"
      }
    }
  }
}
