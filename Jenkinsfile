pipeline {
    agent any

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Master-ciberseguridad/DVWA'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Se asegura que la variable SONAR_AUTH_TOKEN exista
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            sonar-scanner \
                                -Dsonar.projectKey=testPipeLine \
                                -Dsonar.sources=vulnerabilities \
                                -Dsonar.host.url=${env.SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_AUTH_TOKEN} \
                                -Dsonar.php.version=8.0
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
            echo 'Pipeline finalizado con éxito.'
        }
        failure {
            echo 'Pipeline falló. Revisa los logs para más detalles.'
        }
        always {
            echo 'Pipeline terminado.'
        }
    }
}
