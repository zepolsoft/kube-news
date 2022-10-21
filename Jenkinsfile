pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dokerapp = docker.build("zepolsoft/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}