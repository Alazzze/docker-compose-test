pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    git 'https://github.com/Alazzze/wordpress-docker-phpmyadmin.git'
                }
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    sh 'docker-compose -f https://raw.githubusercontent.com/Alazzze/wordpress-docker-phpmyadmin/main/docker-compose.yml up --build'
                }
            }
        }
    }
}
