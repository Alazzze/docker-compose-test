pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    try {
                        sh 'docker-compose down -v'
                        sh 'docker-compose up -d' // Додана команда для запуску контейнерів
                        sh 'docker-compose logs' // Виведення логів контейнерів
                    } catch (Exception ex) {
                        echo "Error occurred while stopping Docker containers: ${ex.message}"
                        // Продовжуйте виконання, навіть якщо відбулася помилка
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    sh 'docker-compose down -v'
                } catch (Exception ex) {
                    echo "Error occurred while stopping Docker containers: ${ex.message}"
                    // Продовжуйте виконання, навіть якщо відбулася помилка
                }
            }
        }
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }
}
