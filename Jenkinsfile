pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "rafisuqbhi/kantin-backend:latest"
        FRONTEND_IMAGE = "rafisuqbhi/kantin-frontend:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/RafiSubqhi/prakdpcc.git'
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t $FRONTEND_IMAGE ./frontend'
            }
        }

        stage('Push Docker') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $BACKEND_IMAGE
                    docker push $FRONTEND_IMAGE
                    '''
                }
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh 'kubectl apply -f kantin-k8s.yaml.yml'
            }
        }
    }
}
