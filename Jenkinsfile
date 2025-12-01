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
                echo "Checking Docker & Compose..."
                sh 'docker --version'
                sh 'docker-compose --version'
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

        // ❌ TEST STAGE DISABLED
        /*
        stage('Run Tests (Optional)') {
            steps {
                echo "Running tests..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} exec cvat pytest -v || echo 'Tests failed'"
            }
        }
        */

        // ❌ STOP CONTAINERS DISABLED
        /*
        stage('Stop CVAT (Optional)') {
            steps {
                echo "Stopping CVAT containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
            }
        }
        */
    }

    // ❌ POST CLEANUP DISABLED
    /*
    post {
        always {
            echo "Cleaning up..."
            sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down -v || true"
        }
    }
    */
}
