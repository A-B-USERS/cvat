pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        CVAT_HOST = 'localhost'  // yahan apna host ya IP daal sakte ho
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo "Cloning CVAT repository..."
                checkout([$class: 'GitSCM',
                    branches: [[name: 'develop']],
                    userRemoteConfigs: [[url: 'https://github.com/A-B-USERS/cvat.git']]
                ])
            }
        }

        stage('Check Docker & Compose') {
            steps {
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

        stage('Start CVAT') {
            steps {
                echo "Starting CVAT containers..."
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
            }
        }

        stage('Verify Containers') {
            steps {
                echo "Listing running containers..."
                sh "docker ps"
            }
        }

        stage('Optional Cleanup') {
            steps {
                script {
                    def doCleanup = input(id: 'confirm', message: 'Do you want to stop and remove CVAT containers?', parameters: [booleanParam(defaultValue: false, description: 'Check to cleanup', name: 'Cleanup')])
                    if (doCleanup) {
                        sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down -v"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
