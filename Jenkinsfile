pipeline {
    agent any

    environment {
        IMAGE_NAME = "skdevops888/foodfiesta"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/skdevops888/Food-Fiesta.git'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t foodfiesta:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Tag') {
            steps {
                sh 'docker tag foodfiesta:${BUILD_NUMBER} $IMAGE_NAME:${BUILD_NUMBER}'
                sh 'docker tag foodfiesta:${BUILD_NUMBER} $IMAGE_NAME:latest'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'bb21d6aa-1b64-4ddd-84f0-fbc126ad660b',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:${BUILD_NUMBER}
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
