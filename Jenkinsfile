pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        DOCKER_HUB_USER = "hannahantony"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 $DOCKER_HUB_USER/$IMAGE_NAME:latest || true'
            }
        }
    }
}