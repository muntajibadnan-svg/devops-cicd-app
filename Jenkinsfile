pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/muntajibadnan-svg/devops-cicd-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t mohammed314/devops-cicd-app:latest .'
            }
        }

        stage('Test') {
            steps {
                sh 'sudo docker run --rm mohammed314/devops-cicd-app:latest python -c "import flask; print(flask.__version__)"'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | sudo docker login -u "$DOCKER_USER" --password-stdin
                        sudo docker push mohammed314/devops-cicd-app:latest
                    '''
                }
            }
        }
    }
}
