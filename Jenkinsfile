pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sanketf5/todo-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/SANKET-tech22/todo-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:v1 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                usernameVariable: 'USER', passwordVariable: 'PASS')]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE:v1'
                }
            }
        }

    }
}