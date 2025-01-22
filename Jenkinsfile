pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'my-react-app'
        DOCKER_REGISTRY = 'my-docker-registry.example.com'
        DOCKER_REGISTRY_CREDENTIALS_ID = 'docker-registry-credentials'
        DEPLOYMENT_SERVER_IP = 'your.server.ip.address'
        DEPLOYMENT_PATH = '/var/www/my-react-app'
        SSH_CREDENTIALS_ID = 'ssh-credentials-id'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                git url: '', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE_NAME}:latest")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Placeholder for any tests
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker registry
                    docker.withRegistry("https://${DOCKER_REGISTRY}", "${DOCKER_REGISTRY_CREDENTIALS_ID}") {
                        // Push Docker image to registry
                        docker.image("${DOCKER_IMAGE_NAME}:latest").push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker image to the server
                    sh '''
                        ssh -i ${SSH_CREDENTIALS_ID} user@${DEPLOYMENT_SERVER_IP} '
                            docker pull ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:latest &&
                            docker stop my-running-container || true &&
                            docker rm my-running-container || true &&
                            docker run -d --name my-running-container -p 80:80 ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:latest
                        '
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup workspace after build
            cleanWs()
        }
        success {
            // Actions on successful deployment
            echo 'Deployment successful!'
        }
        failure {
            // Actions on failed deployment
            echo 'Deployment failed!'
        }
    }
}
