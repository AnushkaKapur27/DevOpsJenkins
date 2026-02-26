pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "anushkakapur2023bcs0149"
        IMAGE_NAME = "frontend"
        FULL_IMAGE_NAME = "${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
        CONTAINER_NAME = "anushkabcs149-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $FULL_IMAGE_NAME .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $FULL_IMAGE_NAME'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:80 --name $CONTAINER_NAME $FULL_IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo "Image pushed to DockerHub and container deployed!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
