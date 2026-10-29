pipeline {
    agent any

    environment {
        IMAGE = 'krithika2013/k8s-practical-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                bat 'py -m pip install -r requirements.txt'
                bat 'py -m pytest -v'
            }
        }

        stage('Build and Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    bat 'docker login -u "%DOCKER_USER%" -p "%DOCKER_TOKEN%"'
                    bat 'docker build -t %IMAGE%:%BUILD_NUMBER% .'
                    bat 'docker push %IMAGE%:%BUILD_NUMBER%'
                    bat 'docker logout'
                }
            }
        }
    }
}