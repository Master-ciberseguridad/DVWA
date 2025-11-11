pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
        SONAR_HOST_URL = 'http://10.30.212.28:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Aquí usamos withCredentials para inyectar el token como variable de entorno
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    // El withSonarQubeEnv debe estar **dentro** del withCredentials
                    withSonarQubeEnv("${SONARQUBE_SERVER}") {
                        sh """
                            echo "Token utilizado: \$SONAR_AUTH_TOKEN"
                            sonar-scanner \\
                            -Dsonar.projectKey=tu_proyecto \\
                            -Dsonar.sources=. \\
                            -Dsonar.host.url=${SONAR_HOST_URL} \\
                            -Dsonar.login=\$SONAR_AUTH_TOKEN
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
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
