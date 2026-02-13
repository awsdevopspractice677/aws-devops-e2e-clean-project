pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "408676698146.dkr.ecr.ap-south-1.amazonaws.com/demo-app"
        IMAGE_TAG = "1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
                    sh 'docker build -t demo-app:${IMAGE_TAG} .'
                }
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION |
                docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Tag & Push Image') {
            steps {
                sh '''
                docker tag demo-app:${IMAGE_TAG} $ECR_REPO:${IMAGE_TAG}
                docker push $ECR_REPO:${IMAGE_TAG}
                '''
            }
        }
	stage('Deploy to ECS') {
    steps {
        sh '''
        aws ecs update-service \
          --cluster demo-app-cluster \
          --service demo-app-service \
          --force-new-deployment \
          --region $AWS_REGION
        '''
    }
   }

    }
}

