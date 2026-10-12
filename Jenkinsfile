pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mohammed314/devops-cicd-app'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/muntajibadnan-svg/devops-cicd-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "sudo docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                sh "sudo docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo ${DOCKER_PASS} | sudo docker login -u ${DOCKER_USER} --password-stdin"
                    sh "sudo docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "sudo docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh "sudo docker stop devops-cicd-app || true"
                sh "sudo docker rm devops-cicd-app || true"
                sh "sudo docker run -d --name devops-cicd-app -p 5000:5000 ${DOCKER_IMAGE}:latest"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
