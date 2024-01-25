pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE_VERSION = '1.29.0'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git url: 'https://github.com/Alazzze/wordpress-docker-phpmyadmin.git', branch: 'main'
                }
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    // Use the Docker Compose version specified in the environment
                    sh "docker-compose -v || curl -L https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-\$(uname -s)-\$(uname -m) -o /usr/local/bin/docker-compose"
                    sh 'chmod +x /usr/local/bin/docker-compose'

                    // Copy .env.example to .env
                    sh 'cp .env.example .env'

                    // Build and start containers
                    sh 'docker-compose up --build'
                }
            }
        }
    }
}
