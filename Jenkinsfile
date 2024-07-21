pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hello-world-app"
        DOCKER_REGISTRY = "tauqeerdocker" // Docker Hub registry
        DOCKER_HUB_USERNAME = "tauqeerahmad104@gmail.com" // Replace with your Docker Hub username
        DOCKER_HUB_CREDENTIALS_ID = ""
        KUBECONFIG_CREDENTIALS_ID = "k8s-service-account"
        KUBERNETES_NAMESPACE = "jenkins" // or your specific namespace
    }

    stages {
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", DOCKER_HUB_CREDENTIALS_ID) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: KUBECONFIG_CREDENTIALS_ID]) {
                        sh 'kubectl apply -f deployment.yaml -n ${KUBERNETES_NAMESPACE}'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
