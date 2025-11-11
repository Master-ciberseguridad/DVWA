pipeline {
    agent any

    environment {
        // Variables normales
        SONARQUBE_SERVER = 'SonarQube'
        SONAR_HOST_URL = 'http://10.30.212.28:9000'
    }

    stages {

        stage('Checkout') {
            steps {
                // Clonar el código fuente
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Usamos withCredentials para inyectar el token Secret Text
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    withSonarQubeEnv("${SONARQUBE_SERVER}") {
                        sh """
                            sonar-scanner \\
                            -Dsonar.projectKey=tu_proyecto \\
                            -Dsonar.sources=. \\
                            -Dsonar.host.url=${SONAR_HOST_URL} \\
                            -Dsonar.login=${SONAR_AUTH_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Esperar el resultado del Quality Gate
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completado exitosamente."
        }
        failure {
            echo "Pipeline falló. Revisa los logs para más detalles."
        }
    }
}
