pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/awsdevopspractice677/aws-devops-e2e-clean-project.git'
            }
        }

        stage('Build Maven') {
            steps {
                dir('app/demo-app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app/demo-app') {
                    sh 'docker build -t demo-app:1.0 .'
                }
            }
        }
    }
}

