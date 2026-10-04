pipeline {
    agent any
    
    environment {
        CONTAINER_NAME = "nestjs-app"
        IMAGE_NAME     = "nestjs-image"
        EMAIL          = "rohantade22@gmail.com"
        PORT           = "3000"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Double quotes allow Groovy to interpolate, with '.'  for build context
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop & Remove Previous Container') {
            steps {
                // -f (force) stops and removes in a single step safely
                sh "docker rm -f ${CONTAINER_NAME} || true"
            }
        }

        stage('Run Docker Container') {
            steps {
                // Kept on one line or joined with \ to avoid command splitting
                sh "docker run -d -p ${PORT}:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }

        stage('Send Email  Notification') {
            steps {
                // Correct step name is 'emailext' (Email Extension Plugin)
                emailext(
                    to: "${EMAIL}",
                    subject: "NestJS Application Deployed Successfully on EC2!",
                    body: "Your NestJS application has been successfully deployed! Visit: http://3.26.18.6:${PORT}/"
                )
            }
        }
    }
}