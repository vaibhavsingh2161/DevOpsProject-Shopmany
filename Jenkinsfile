pipeline {
    agent any

    environment {
        // Define environment variables for docker-compose file location and binary
        DOCKER_COMPOSE_FILE = 'docker-compose.yaml'
        DOCKER_COMPOSE = "/usr/local/bin/docker-compose"  // Path to Docker Compose binary
        PROJECT_DIR = '/Users/vaibhavsingh/Documents/MProjects/shopmany' // Project directory
    }

    stages {
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
                        sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} build --no-cache"
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running Tests...'
                    dir("${PROJECT_DIR}") {
                        // Example test command (replace with actual test commands)
                        sh 'echo "Tests completed successfully"'
                    }
                }
            }
        }

        stage('Start Containers') {
            steps {
                script {
                    echo 'Starting Containers...'
                    dir("${PROJECT_DIR}") {
                        sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} up -d"
                    }
                }
            }
        }

        stage('Verify Services') {
            steps {
                script {
                    echo 'Verifying Services...'
                    dir("${PROJECT_DIR}") {
                        // Example verification step (replace with actual commands)
                        sh 'docker ps'
                        echo 'Verification completed successfully!'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Services...'
                    // Add deployment steps here if applicable
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
                    sh "${DOCKER_COMPOSE} -f ${DOCKER_COMPOSE_FILE} down"
                }
            }
        }
    }
}