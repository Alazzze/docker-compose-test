pipeline {
    agent any
    
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
                    sh 'cp .env.example .env'  // Копіюємо .env.example в .env
                    sh 'docker-compose -f docker-compose.yml up --build'
                }
            }
        }
    }
}
