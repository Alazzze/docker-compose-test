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
                        sh 'docker-compose -f docker-compose.yml down -v'
                    } catch (Exception ex) {
                        echo "Error occurred while stopping Docker containers: ${ex.message}"
                        // Додайте необхідні дії для обробки помилки
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    sh 'docker-compose -f docker-compose.yml down -v'
                } catch (Exception ex) {
                    echo "Error occurred while stopping Docker containers: ${ex.message}"
                    // Додайте необхідні дії для обробки помилки
                }
            }
        }
    }

    options {
        timeout(time: 1, unit: 'HOURS') // Додайте тайм-аут для уникнення вічних зависань
    }
}
