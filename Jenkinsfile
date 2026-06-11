pipeline {
    agent any
    
    tools {
        jdk 'JDK 17'
        maven 'Maven 3'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        buildDiscarder(logRotate(numToKeepStr: '10', artifactsNumToKeepStr: '5'))
        timestamps()
        skipDefaultCheckout()
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
                sshagent(credentials:['server-key-id']) {
                    sh '''
                        ssh ruben2cc@34.70.105.81 "
                            pwd
                            sudo systemctl status huaspro || true
                        "
                    '''
                }
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
        stage('Cuando es Pull Request') {
            when {
                changeRequest()
            }
            steps {
                echo "Se ejecutó el stage de Pull request"
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}