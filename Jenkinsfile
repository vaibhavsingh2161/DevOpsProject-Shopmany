pipeline {
    agent any

    environment {
        // Define environment variables for docker-compose file location and binary
        DOCKER_COMPOSE_FILE = 'docker-compose.yaml'
        DOCKER_COMPOSE = "/usr/local/bin/docker-compose"  // Path to Docker Compose
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the source control (GitHub)
                    checkout scm
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build all Docker images based on the docker-compose.yaml file
                    echo "Building Docker images..."
                    sh '"$DOCKER_COMPOSE" -f $DOCKER_COMPOSE_FILE build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run your tests here if applicable, using Docker containers
                    // Example: Test your backend services if you have test suites
                    // sh '"$DOCKER_COMPOSE" -f $DOCKER_COMPOSE_FILE run frontend npm test'
                    echo 'Running Tests...'
                }
            }
        }

        stage('Start Containers') {
            steps {
                script {
                    // Start the containers (detached mode)
                    echo 'Starting Containers...'
                    sh '"$DOCKER_COMPOSE" -f $DOCKER_COMPOSE_FILE up -d'
                }
            }
        }

        stage('Verify Services') {
            steps {
                script {
                    // Verify the services are running (example: check if an endpoint is up)
                    echo 'Verifying Services...'
                    // Add logic here to verify services, for example, by using curl or another check
                    // sh 'curl http://localhost:YOUR_PORT/health'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy your services if needed
                    echo 'Deploying Services...'
                    // Example for pushing images or deploying to cloud services
                    // sh 'docker push my_image'
                    // or
                    // sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check logs for errors.'
            // You can also add cleanup steps here if needed
        }
    }
}
