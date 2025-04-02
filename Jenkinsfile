pipeline {
    agent any

    environment {
        CONTAINER_NAME = "ecommerce"
        DOCKER_CRED = "docker_creds"
        DOCKER_IMAGE = "kpkm25/jenkins-ecommerce"
    }

    stages {
        stage("Git Clone") {
            steps {
                script {
                    echo "Cloning the repository..."
                    git url: "https://github.com/KPkm25/jenkins-ecommerce", branch: "main"
                }
            }
        }

        stage("Docker Build") {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage("Docker Push") {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub..."
                    docker.withRegistry('', DOCKER_CRED) {
                        echo "Successful login"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage("Local Deployment") {
            steps {
                script {
                    echo "Deploying Docker container locally..."
                    sh """
                        docker pull ${DOCKER_IMAGE}:latest
                        docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker stop || true
                        docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker rm || true
                        docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
