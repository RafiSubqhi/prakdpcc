pipeline {
    agent any
    environment {
        DOCKER_USER = "rafisuqbhi"
        DOCKER_CREDS = 'dockerhub'
        KUBE_CREDS = 'kubeconfig'
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}", passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        
                        // Build & Push Backend - Pastikan folder 'backend' ada di repo
                        sh "docker build -t ${DOCKER_USER}/kantin-backend:latest ./backend"
                        sh "docker push ${DOCKER_USER}/kantin-backend:latest"
                        
                        // Build & Push Frontend - Pastikan folder 'frontend' ada di repo
                        sh "docker build -t ${DOCKER_USER}/kantin-frontend:latest ./frontend"
                        sh "docker push ${DOCKER_USER}/kantin-frontend:latest"
                    }
                }
            }
        }
        stage('Deploy to AKS') {
            steps {
                withCredentials([file(credentialsId: "${KUBE_CREDS}", variable: 'KUBE')]) {
                    sh "kubectl --kubeconfig=${KUBE} apply -f kantin-k8s.yaml.yml"
                    sh "kubectl --kubeconfig=${KUBE} apply -f kantin-ingress.yaml"
                }
            }
        }
    }
}
