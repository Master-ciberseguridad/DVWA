pipeline { 
    agent any 
    environment { 
        // Nombre del servidor SonarQube configurado en Jenkins 
        SONARQUBE_SERVER = 'SonarQube' 
        SONAR_HOST_URL = 'http://10.30.212.28:9000' 
        SONAR_AUTH_TOKEN = credentials('sonar-token') 
    }
        stages { 
            stage('Checkout') { 
                steps {
                    // Clonar el código fuente desde el repositorio checkout scm 
                } 
            }
            stage('SonarQube Analysis') { 
                steps { 
                    // Configurar el entorno de SonarQube 
                    withSonarQubeEnv("${SONARQUBE_SERVER}") { 
                        // Ejecutar el análisis con SonarScanner 
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
            stage('Quality Gate') { 
                steps { 
                    // Esperar el resultado del Quality Gate timeout(time: 1, unit: 'HOURS') 
                    { 
                        waitForQualityGate abortPipeline: true 
                    }
                }
            }
        }
    }

