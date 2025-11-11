pipeline {
    agent any

    environment {
        // Nombre del servidor SonarQube configurado en Jenkins
        SONARQUBE_SERVER = 'SonarQube'
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
                sh "${tool 'SonarScanner'}/bin/sonar-scanner -Dsonar.projectKey=testPipeLine -Dsonar.sources=vulnerabilities -Dsonar.php.version=8.0"
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
