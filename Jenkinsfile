pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'docker compose build'
            }
        }

        stage('Run') {
            steps {
                bat 'docker compose up -d'
            }
        }

        stage('Test') {
            steps {
                bat 'curl http://localhost:5000/api/hello'
            }
        }
    }
}