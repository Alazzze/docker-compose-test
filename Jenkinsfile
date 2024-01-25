pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE_VERSION = "1.29.0"  // Версія Docker Compose
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
                    sh '''
                        docker-compose -v || {
                            curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose
                            chmod +x docker-compose
                            export PATH=$PATH:/var/lib/jenkins/workspace/test5
                        }
                        docker-compose -f docker-compose.yml up --build
                    '''
                }
            }
        }
    }
}
