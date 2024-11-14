pipeline {
    agent any

    environment {
        // Define any environment variables you need here
        DOCKER_COMPOSE_FILE = 'shopmany/docker-compose.yaml'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from GitHub (or your source control)
                    checkout scm
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build all Docker images based on the docker-compose.yaml file
                    sh 'docker-compose -f $DOCKER_COMPOSE_FILE build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run your tests here if applicable, using Docker containers
                    // Example: Test your backend services if you have test suites
                    // sh 'docker-compose -f $DOCKER_COMPOSE_FILE run frontend npm test'
                    
                    // Example to run unit tests or integration tests if you have them
                    // You can customize this step based on the service you are testing
                    echo 'Running Tests...'
                }
            }
        }

        stage('Start Containers') {
            steps {
                script {
                    // Start the containers (this won't detach them, for debugging purposes)
                    sh 'docker-compose -f $DOCKER_COMPOSE_FILE up -d'
                }
            }
        }

        stage('Verify Services') {
            steps {
                script {
                    // Verify the services are running (example: hit an endpoint)
                    // Add your verification logic, for example using curl to check if services are up
                    echo 'Verifying Services...'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy your services if needed
                    // This could involve pushing the built Docker images to a registry
                    // or deploying the services to a cloud provider
                    echo 'Deploying Services...'
                }
            }
        }

        
    }

    post {
        always {
            // Clean up any resources even if the pipeline fails
            echo 'Cleaning up...'
            sh 'docker-compose -f $DOCKER_COMPOSE_FILE down'
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
