pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker')
        IMAGE_NAME_SERVER = 'ihebbac/mern-app-main-server'
        IMAGE_NAME_CLIENT = 'ihebbac/mern-app-main-client'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:Ihebbac/devops_mern_app.git',
                    credentialsId: '52243a7a-c599-4e56-9b2a-a617079c5ff6'
            }
        }

        stage('Build Server Image') {
            steps {
                dir('server') {
                    script {
                        dockerImageServer = docker.build("${IMAGE_NAME_SERVER}")
                    }
                }
            }
        }
        stage('Build Client Image') {
            steps {
                dir('client') {
                    script {
                        dockerImageClient = docker.build("${IMAGE_NAME_CLIENT}")
                    }
                }
            }
        }
        stage('Scan Server Image') {
            steps {
                script {
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    ${IMAGE_NAME_SERVER}
                    """
                }
            }
        }
        stage('Scan Client Image') {
            steps {
                script {
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    ${IMAGE_NAME_CLIENT}
                    """
                }
            }
        }
        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        dockerImageServer.push()
                        dockerImageClient.push()
                    }
                }
            }
        }
    }
}
