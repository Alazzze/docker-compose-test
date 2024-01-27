pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    try {
                        sh 'docker-compose down -v'
                        sh 'docker-compose build'
                        sh 'docker-compose up -d'

                        // Вивести інформацію про версії образів
                        sh 'docker-compose version'

                        // Почекати деякий час для того, щоб сервіси успішно запустилися
                        sleep time: 30, unit: 'SECONDS'

                        // Перевірити стан контейнерів перед завершенням
                        def containersRunning = sh(script: 'docker ps --format "{{.Names}}"', returnStatus: true).isSuccess()
                        if (containersRunning) {
                            echo 'All containers are running.'
                        } else {
                            error 'Some containers failed to start.'
                        }
                    } catch (Exception ex) {
                        echo "Error occurred: ${ex.message}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }

    post {
        always {
            // Завжди виконується, навіть якщо є помилка
            script {
                // Зупинити та видалити контейнери після завершення тестів
                sh 'docker-compose down -v'
            }
        }

        success {
            // Виконується, якщо пайплайн завершується успішно
            echo 'Deployment and testing successful!'
        }

        failure {
            // Виконується, якщо пайплайн завершується з помилкою
            echo 'Deployment or testing failed!'
        }
    }
}
