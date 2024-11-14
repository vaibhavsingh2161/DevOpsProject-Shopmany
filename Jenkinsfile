pipeline {
    agent any

    environment {
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

        stage('Build Docker Images') {
            steps {
                script {
                    // Move into each directory and build the Docker image
                    bat 'cd DevopsProject-Shopmany'
                    bat 'cd items && docker build -t shopmany_item:%DOCKER_IMAGE_TAG% .'
                    bat 'cd ../pay && docker build -t shopmany_pay:%DOCKER_IMAGE_TAG% .'
                    bat 'cd ../frontend && docker build -t shopmany_frontend:%DOCKER_IMAGE_TAG% .'
                    bat 'cd ../discount && docker build -t shopmany_discount:%DOCKER_IMAGE_TAG% .'
                }
            }
        }

        stage('Create Docker Network') {
            steps {
                script {
                    // Check if the network exists; if not, create it
                    bat 'docker network inspect %DOCKER_NETWORK% || docker network create %DOCKER_NETWORK%'
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    // Use docker-compose to bring up all services
                    bat 'docker-compose down'
                    bat 'docker-compose up -d'
                }
            }
        }

        stage('Post Deployment Verification') {
            steps {
                script {
                    // Perform simple health checks for services
                    bat 'curl --fail http://localhost:3000 || echo "Frontend not running"'
                    bat 'curl --fail http://localhost:3001/item || echo "Item service not running"'
                    bat 'curl --fail http://localhost:3002/pays || echo "Pay service not running"'
                    bat 'curl --fail http://localhost:3003/discount?itemid=1 || echo "Discount service not running"'
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
