pipeline {
    agent any

    environment {
        ECR_REPO = "itai1/app"   
        AWS_REGION = "us-east-1"     
        AWS_ACCOUNT_ID = "992382545251"  
        IMAGE_TAG = "pr-${env.CHANGE_ID}-${BUILD_NUMBER}"  
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    // BUILDING DOCKER CONTAINER  
                    sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // CI unit test
                    sh 'docker run --rm $ECR_REPO:$IMAGE_TAG npm test'  // אם אתה עובד עם Node.js למשל
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Connecting to the ECR
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO'

                    // pushing the image to the amazon ECR
                    sh 'docker tag $ECR_REPO:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG'
                    sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG'
                }
            }
        }
    }

    post {
        always {
            cleanWs() // לנקות את ה-workspace בסוף
        }
    }
}
