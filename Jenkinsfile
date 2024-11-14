pipeline {
    agent any

    environment {
        DOCKER_NETWORK = "gaworkshop"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }

        stage('Create Docker Network') {
            steps {
                script {
                    // Check if the network exists, if not create it
                    bat 'docker network inspect %DOCKER_NETWORK% || docker network create %DOCKER_NETWORK%'
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    // Use docker-compose to bring up all services from the docker-compose.yml file
                    bat 'docker-compose down'    // Stop any running services (clean up)
                    bat 'docker-compose up -d'   // Start services in detached mode
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
