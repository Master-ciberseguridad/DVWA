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
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    withCredentials([string(credentialsId: 'sonarQ-token', variable: 'SONAR_AUTH_TOKEN')]) {
                        sh """
                            sonar-scanner \\
                                -Dsonar.projectKey=tu_proyecto \\
                                -Dsonar.sources=. \\
                                -Dsonar.host.url=${SONAR_HOST_URL} \\
                                -Dsonar.login=\$SONAR_AUTH_TOKEN \\
                                -Dsonar.inclusions=**/*.php,**/*.py \\
                                -Dsonar.exclusions=vendor/**,node_modules/**,**/*.js
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
}
