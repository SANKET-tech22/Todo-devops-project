pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sanketf5/app_app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                export KUBECONFIG=/home/ubuntu/.kube/config
                sh 'kubectl apply -f k8s/'
                sh 'kubectl set image deployment/todo-app todo-app=$DOCKER_IMAGE:$BUILD_NUMBER'
                '''
            }
        }
    }
}