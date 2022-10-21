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
            environment {
                tag_version = "${env.BUID_ID}"
            }

            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/Deployment.yaml'
                    sh 'kubectl apply -f ./k8s/Deployment.yaml'
                }
            }
        }
    }
}
