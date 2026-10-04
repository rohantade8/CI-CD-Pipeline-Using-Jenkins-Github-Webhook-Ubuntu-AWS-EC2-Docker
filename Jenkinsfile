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
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop & Remove Previous Container') {
            steps {
                sh "docker rm -f ${CONTAINER_NAME} || true"
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p ${PORT}:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            emailext(
                to: "${EMAIL}",
                from: "${EMAIL}",
                replyTo: "${EMAIL}",
                subject: "Jenkins Build #${BUILD_NUMBER} - Deployment Successful",
                body: """
Jenkins Deployment Successful

Job: ${JOB_NAME}
Build: #${BUILD_NUMBER}
Status: SUCCESS

NestJS application has been successfully deployed on AWS EC2.

Application:
http://3.26.18.6:${PORT}/

Jenkins Build:
${BUILD_URL}
""",
                mimeType: 'text/plain'
            )
        }
    }
}