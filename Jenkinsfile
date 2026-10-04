pipeline{
    agent any
    
    environment {
        CONTAINER_NAME = "nestjs-app"
        IMAGE_NAME = "nestjs-image"
        EMAIL = "rohan@gmail.com"
        PORT = "3000"
    }

    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'main', url: 'https://github.com/rohantade8/CI-CD-Pipeline-Using-Jenkins-Github-Webhook-Ubuntu-AWS-EC2-Docker.git'
    
            }
        }

        stage('Build Docker Image'){
            steps{
                sh 'docker build -t $IMAGE_NAME'
            }
        }

        stage('Stop & Remove Previous Container'){
            steps{
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container'){
            steps{
                sh '''
                    docker run -d -p ${PORT}:${PORT}
                    --name $CONTAINER_NAME $IMAGE_NAME
                '''
                }
        }

        stage('Send Email Notification'){
            steps{
                emailtext(
                    subject: "Next.js Application Deployed Successful on EC2!",
                    body: "Your Next.js application has been successfully deployed! http://3.26.18.6:${PORT}/",
                    to : "${EMAIL}"
                )
            }
        }
    }
}