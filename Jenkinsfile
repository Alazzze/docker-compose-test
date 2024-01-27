pipeline {
    agent any

    environment {
        GIT_TOOL = tool name: 'Default', type: 'hudson.plugins.git.GitTool'
        DOCKER_TOOL = tool name: 'Default', type: 'hudson.plugins.docker.commons.tools.DockerTool'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    withEnv(["GIT_HOME=${env.GIT_TOOL}/bin", "PATH+GIT=${env.GIT_TOOL}/bin"]) {
                        checkout scm
                    }
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    withEnv(["DOCKER_HOME=${env.DOCKER_TOOL}/bin", "PATH+DOCKER=${env.DOCKER_TOOL}/bin"]) {
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
