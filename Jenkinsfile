pipeline {
    agent any

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
                echo 'Se ha compilado el proyecto'
            }
        }
    }
}