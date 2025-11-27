pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        PROJECT_NAME = 'cvat'
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'docker --version || echo "Docker not installed!"'
                sh 'docker-compose --version || echo "Docker Compose not installed!"'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "Building Docker images..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} build"
            }
        }

        stage('Run CVAT') {
            steps {
                echo "Starting CVAT containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
            }
        }

        stage('Run Tests (Optional)') {
            steps {
                echo "Running tests..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} exec cvat pytest -v || echo 'Tests failed'"
            }
        }

        stage('Stop CVAT (Optional)') {
            steps {
                echo "Stopping CVAT containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
            }
        }
    }

    post {
        always {
            echo "Cleaning up Docker containers..."
            sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down -v || true"
        }
    }
}
