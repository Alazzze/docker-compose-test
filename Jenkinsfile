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
                    // Зупинка і видалення контейнерів
                    sh 'docker-compose down -v || true'  // Добавляємо `|| true`, щоб продовжити виконання, навіть якщо виникає помилка

                    // Побудова та запуск контейнерів
                    try {
                        sh 'docker-compose build'
                        sh 'docker-compose up -d'
                        sleep time: 30, unit: 'SECONDS'
                        def containersRunning = sh(script: 'docker ps --format "{{.Names}}"', returnStatus: true).isSuccess()
                        if (containersRunning) {
                            echo 'All containers are running.'
                        } else {
                            error 'Some containers failed to start.'
                        }
                    } catch (Exception ex) {
                        echo "Error occurred while building and starting Docker containers: ${ex.message}"
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
                // Зупинка і видалення контейнерів
                sh 'docker-compose down -v || true'  // Добавляємо `|| true`, щоб продовжити виконання, навіть якщо виникає помилка
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
