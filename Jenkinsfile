pipeline {
    agent any

    tools {
        maven 'Maven 3'
        jdk 'JDK 17'
    }

    environment {
        APP_NAME = 'mi-app'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Descargando código...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compilando aplicación... que no es maven jeje...'
                sshagent(credentials: ['server-key-id']) {
                    sh 
                    '''
                        whoami
                        pwd
                    '''
                }
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Generando artefacto...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archivar') {
            steps {
                echo 'Guardando artefacto...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline ejecutado correctamente para ${APP_NAME}"
        }

        failure {
            echo "El pipeline falló. Revisa los logs."
        }

        always {
            echo 'Pipeline finalizado.'
        }
    }
}