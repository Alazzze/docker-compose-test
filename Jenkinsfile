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
                    } catch (Exception ex) {
                        echo "Error occurred while stopping Docker containers: ${ex.message}"
                        currentBuild.result = 'FAILURE'
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
                    currentBuild.result = 'FAILURE'
                }
            }
        }
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }
}
