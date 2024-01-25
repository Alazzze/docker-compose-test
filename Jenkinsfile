pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Команда для витягування коду з репозиторію
                    git 'git@github.com:Alazzze/wordpress-docker-phpmyadmin.git'
                }
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    // Команда для тестування за допомогою Docker Compose
                    sh 'docker-compose -f https://raw.githubusercontent.com/Alazzze/wordpress-docker-phpmyadmin/main/docker-compose.yml up --build'
                }
            }
        }
    }
}
