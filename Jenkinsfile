pipeline {
agent any
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

    stage('Docker Images') {
        steps {
            sh 'docker images | grep foodfiesta'
        }
    }
}

}
