pipeline {
    agent any

    environment {
        // Set environment variables needed for the build
        DOCKER_NETWORK = "gaworkshop"
        DOCKER_IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }

        stage('Build & Push Docker Images') {
            steps {
                script {
                    // Build each Docker image and tag them
                    sh 'docker build -t shopmany_item:${DOCKER_IMAGE_TAG} ./items'
                    sh 'docker build -t shopmany_pay:${DOCKER_IMAGE_TAG} ./pay'
                    sh 'docker build -t shopmany_frontend:${DOCKER_IMAGE_TAG} ./frontend'

                    // Push the images to a Docker registry if needed
                    // Uncomment the lines below if you are using a Docker registry
                    // sh 'docker login -u <username> -p <password>'
                    // sh 'docker tag shopmany_item:${DOCKER_IMAGE_TAG} <registry>/shopmany_item:${DOCKER_IMAGE_TAG}'
                    // sh 'docker push <registry>/shopmany_item:${DOCKER_IMAGE_TAG}'
                }
            }
        }

        stage('Create Docker Network') {
            steps {
                script {
                    // Check if the network exists, if not create it
                    sh 'docker network inspect ${DOCKER_NETWORK} || docker network create ${DOCKER_NETWORK}'
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    // Use docker-compose to bring up all services
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Post Deployment Verification') {
            steps {
                script {
                    // Perform simple health checks for services
                    sh 'curl --fail http://localhost:3000 || echo "Frontend not running"'
                    sh 'curl --fail http://localhost:3001/item || echo "Item service not running"'
                    sh 'curl --fail http://localhost:3002/pays || echo "Pay service not running"'
                    sh 'curl --fail http://localhost:3003/discount?itemid=1 || echo "Discount service not running"'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for more details.'
        }
    }
}
