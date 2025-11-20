node {
    withEnv([
        "DEPLOY_PORT = ${env.BRANCH_NAME == 'main' ? '3000' : '3001'}",
        "CONTAINER_NAME = app-${env.BRANCH_NAME}-instance"
    ]) {
        docker.image('docker:latest').inside("-v /var/run/docker.sock:/var/run/docker.sock") {
            
            pipeline {
                agent none 

                stages {
                    stage('Checkout SCM') {
                        steps {
                            checkout scm
                        }
                    }
                    
                    stage('Build Image') {
                        steps {
                            sh "docker build -t app-image:${env.BRANCH_NAME} ."
                        }
                    }

                    stage('Deploy') {
                        steps {
                            sh "docker stop ${env.CONTAINER_NAME} || true"
                            sh "docker rm ${env.CONTAINER_NAME} || true"
                            sh "docker run -d --name ${env.CONTAINER_NAME} -p ${env.DEPLOY_PORT}:3000 app-image:${env.BRANCH_NAME}"
                        }
                    }
                }
            }
        }
    }
}
