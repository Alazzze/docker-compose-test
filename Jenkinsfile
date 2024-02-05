pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'wordpress-docker-phpmyadmin:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose -f wordpress-docker-phpmyadmin/docker-compose.yml up -d'
            }
        }

        stage('Test') {
            steps {
           
            }
        }
    }

    post {
        success {
            echo 'Deployment and tests passed. Ready for production!'
        }

        failure {
            echo 'Deployment or tests failed. Check the logs for details.'
        }

        always {
            sh 'docker-compose -f wordpress-docker-phpmyadmin/docker-compose.yml down'
        }
    }
}
