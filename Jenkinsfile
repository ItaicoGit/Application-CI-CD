pipeline {
    agent any
    environment {
        REGISTRY = "992382545251.dkr.ecr.us-east-1.amazonaws.com"
        IMAGE_NAME = "myapp"
    }
    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t $REGISTRY/$IMAGE_NAME:${GIT_COMMIT} .'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests here (Example: Unit tests)
                    sh 'pytest'
                }
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $REGISTRY'
                    // Push the Docker image to ECR
                    sh 'docker push $REGISTRY/$IMAGE_NAME:${GIT_COMMIT}'
                }
            }
        }
        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                script {
                    // Deploy to EC2 production (replace with your deployment commands)
                    sh 'ssh -i itai-cicd.pem ec2-user@3.82.38.174 "docker pull $REGISTRY/$IMAGE_NAME:${GIT_COMMIT} && docker run -d -p 80:80 $REGISTRY/$IMAGE_NAME:${GIT_COMMIT}"'
                }
            }
        }
    }
    post {
        always {
            cleanWs()  // Cleanup workspace
        }
    }
}
