pipeline {
    agent any

    environment {
        // Set the Docker CLI experimental feature flag
        DOCKER_CLI_EXPERIMENTAL = 'enabled'

        // Define environment variables for docker-compose file location and binary
        DOCKER_COMPOSE_FILE = 'docker-compose.yaml'
        DOCKER_COMPOSE = '/usr/local/bin/docker-compose'  // Path to Docker Compose binary
        PROJECT_DIR = '/Users/vaibhavsingh/Documents/MProjects/shopmany' // Project directory
    }

    stages {
        stage('Check Jenkins Node Environment') {
            steps {
                script {
                    echo "Checking Jenkins Node Environment..."
                    // Check if Docker is installed and available
                    sh 'docker --version'
                    sh 'docker-compose --version'
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    echo "Checking out source code..."
                    checkout scm
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building Docker images..."
                    dir("${PROJECT_DIR}") {  // Navigate to project directory
                        try {
                            sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} build --no-cache"
                        } catch (Exception e) {
                            echo "Error during Docker image build: ${e.getMessage()}"
                            currentBuild.result = 'FAILURE'
                            throw e
                        }
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running Tests...'
                    dir("${PROJECT_DIR}") {
                        try {
                            // Example test command (replace with actual test commands)
                            sh 'echo "Running tests completed successfully!"'
                        } catch (Exception e) {
                            echo "Error during tests: ${e.getMessage()}"
                            currentBuild.result = 'FAILURE'
                            throw e
                        }
                    }
                }
            }
        }

        stage('Start Containers') {
            steps {
                script {
                    echo 'Starting Containers...'
                    dir("${PROJECT_DIR}") {
                        try {
                            sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} up -d"
                        } catch (Exception e) {
                            echo "Error during container startup: ${e.getMessage()}"
                            currentBuild.result = 'FAILURE'
                            throw e
                        }
                    }
                }
            }
        }

        stage('Verify Services') {
            steps {
                script {
                    echo 'Verifying Services...'
                    dir("${PROJECT_DIR}") {
                        try {
                            sh 'docker ps'
                            echo 'Services verification completed successfully!'
                        } catch (Exception e) {
                            echo "Error during service verification: ${e.getMessage()}"
                            currentBuild.result = 'FAILURE'
                            throw e
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Services...'
                    // Add deployment steps here if applicable
                    // Example: sh 'deploy.sh'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished. Cleaning up...'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
        cleanup {
            script {
                echo 'Stopping and removing containers...'
                dir("${PROJECT_DIR}") {
                    try {
                        sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} down"
                    } catch (Exception e) {
                        echo "Error during cleanup: ${e.getMessage()}"
                    }
                }
            }
        }
    }
}
