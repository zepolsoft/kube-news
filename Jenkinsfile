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

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dokerapp.push('latest')
                        dokerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                withKubeconfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/Deployment.yaml'
                }
            }
        }
    }
}
