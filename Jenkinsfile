pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        PROJECT_NAME = 'cvat'
        CVAT_HOST = '192.168.0.112'  // <-- add your server IP here
    }

    stages {
        stage('Checkout') { steps { checkout scm } }

        stage('Install Dependencies') {
            steps {
                sh 'docker --version || echo "Docker not installed!"'
                sh 'docker-compose --version || echo "Docker Compose not installed!"'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} build"
            }
        }

        stage('Run CVAT') {
            steps {
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
            }
        }
    }

    post {
        always {
            sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down -v || true"
        }
    }
}
