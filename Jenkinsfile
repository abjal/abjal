pipeline {
    agent any
    environment {
        IMAGE_NAME = "509848800168.dkr.ecr.us-east-1.amazonaws.com/test/test"
    }
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/abjal/abjal.git'
            }
        }
        stage('buid') {
            steps {
                sh 'echo building..done'
            }
        }
        stage('buid docker image') {
            steps {
                sh "docker build -t 509848800168.dkr.ecr.us-east-1.amazonaws.com/test/test:${BUILD_NUMBER} ."
            }
        }
        stage('Login to ECR & Push') {
            steps {
                // Inject AWS credentials as environment variables
                withCredentials([usernamePassword(credentialsId: 'awscreds',
                                                  usernameVariable: 'AWS_ACCESS_KEY_ID',
                                                  passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 509848800168.dkr.ecr.us-east-1.amazonaws.com'
                sh "docker push 509848800168.dkr.ecr.us-east-1.amazonaws.com/test/test:${BUILD_NUMBER}"
                }
            }
        }
        stage('deploy to EKS'){
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                     sh 'kubectl apply -f deploy-service.yaml'
                
                }
        }
        }
    }
}
