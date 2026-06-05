pipeline {
    agent any
    
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