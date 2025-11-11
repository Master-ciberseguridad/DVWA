pipeline {
    agent any

    environment {
        // Nombre del servidor SonarQube configurado en Jenkins
        SONARQUBE_SERVER = 'SonarQube'
        SONAR_HOST_URL = 'http://10.30.212.28:9000' // Cambiado
        SONAR_AUTH_TOKEN = credentials('sqa_5bf3b7a6d3e3b37dac81a5fd73e4bc62217b620e')
        // Agregar sonar-scanner al PATH
        PATH = "/opt/sonar-scanner/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el código fuente desde el repositorio
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Configurar el entorno de SonarQube
                sh "${tool 'SonarScanner'}/bin/sonar-scanner \
                      -Dsonar.projectKey=testPipeLine \
                      -Dsonar.sources=vulnerabilities \
                      -Dsonar.host.url=${env.SONAR_HOST_URL} \
                      -Dsonar.php.version=8.0"
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
}
