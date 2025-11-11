pipeline {
    agent any

    environment {
        SONAR_AUTH_TOKEN = credentials('sonarQ-token') // Token de usuario administrador
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=tu_proyecto \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://10.30.212.28:9000 \
                        -Dsonar.login=${SONAR_AUTH_TOKEN} \
                        -Dsonar.inclusions=**/*.php,**/*.py \
                        -Dsonar.exclusions=vendor/**,node_modules/**,**/*.js
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def maxRetries = 3
                    def success = false
                    for (int i = 1; i <= maxRetries; i++) {
                        try {
                            timeout(time: 1, unit: 'HOURS') {
                                def qg = waitForQualityGate()
                                if (qg.status != 'OK') {
                                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                                }
                                success = true
                                break
                            }
                        } catch (err) {
                            echo "Intento ${i} fallido: ${err}"
                            if (i < maxRetries) {
                                echo "Reintentando en 10 segundos..."
                                sleep 10
                            } else {
                                error "No se pudo obtener el Quality Gate después de ${maxRetries} intentos"
                            }
                        }
                    }
                }
            }
        }
    }
}
