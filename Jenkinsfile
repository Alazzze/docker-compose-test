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
                        
                        // Зчитування значень з .env.example
                        def envContent = readFile(".env.example")
                        def envVars = [:]
                        envContent.eachLine { line ->
                            if (line.trim() && !line.startsWith("#")) {
                                def (key, value) = line.split("=", 2)
                                envVars[key.trim()] = value.trim()
                            }
                        }
                        
                        // Запуск контейнерів зі значеннями з .env.example
                        sh """
                            MYSQL_ROOT_PASSWORD=${envVars.MYSQL_ROOT_PASSWORD} \
                            MYSQL_DATABASE=${envVars.MYSQL_DATABASE} \
                            MYSQL_USER=${envVars.MYSQL_USER} \
                            MYSQL_PASSWORD=${envVars.MYSQL_PASSWORD} \
                            WORDPRESS_DB_NAME=${envVars.WORDPRESS_DB_NAME} \
                            WORDPRESS_DB_USER=${envVars.WORDPRESS_DB_USER} \
                            WORDPRESS_DB_PASSWORD=${envVars.WORDPRESS_DB_PASSWORD} \
                            docker-compose up -d
                        """

                        sh 'docker-compose logs -t'
                    } catch (Exception ex) {
                        echo "Error occurred while stopping/starting Docker containers: ${ex.message}"
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
                   
                }
            }
        }
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
    }
}
