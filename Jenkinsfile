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

        stage('deploy') {
            steps {
                echo "Se despliega el servicio al servidor: ${params.AMBIENTE}"
            }
        }
    }
}