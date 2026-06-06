pipeline {
    agent any
    
    tools {
        jdk 'JDK 17'
        maven 'Maven 3'
    }

    parameters {
        choice(name: 'AMBIENTE', choices: ['DEV', 'QA', 'PROD'])
    }

    stages {
        stage('clone') {
            steps {
                checkout scm
            }
        }

        stage('compile') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('deploy a DEV') {
            when {
                environment name: 'AMBIENTE', value: 'DEV'
            }
            steps {
                echo "Se despliega el servicio al servidor: ${params.AMBIENTE}"
                echo "El nombre del job es: ${env.JOB_NAME}"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('deploy a PROD') {
                    when {
                        environment name: 'AMBIENTE', value: 'PROD'
                    }
                    steps {
                        echo "Se despliega el servicio al servidor: ${params.AMBIENTE}"
                        echo "El nombre del job es: ${env.JOB_NAME}"
                        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                    }
                }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}