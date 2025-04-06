pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'patelraj41299/springapp'
        DOCKER_TAG = 'latest'
    }

    stages {

        stage('Docker Login & Pull') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhublogin', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                    sh "docker pull ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Optional: Stop any container already using this image
                    sh """
                        docker ps -q --filter "ancestor=${DOCKER_IMAGE}:${DOCKER_TAG}" | grep . && docker stop \$(docker ps -q --filter "ancestor=${DOCKER_IMAGE}:${DOCKER_TAG}") || true
                    """
                    // Run the new container
                    sh "docker run -d -p 8081:9090 ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
}

