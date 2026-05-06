pipeline {
    agent any
    environment {
        DOCKER_USER = "rafisuqbhi"
        DOCKER_CREDS = 'dockerhub' // ID di Jenkins
        KUBE_CREDS = 'kubeconfig'   // ID di Jenkins
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}", passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        
                        // Build & Push Backend
                        sh "docker build -t ${DOCKER_USER}/kantin-backend:latest ./backend"[cite: 2, 4]
                        sh "docker push ${DOCKER_USER}/kantin-backend:latest"
                        
                        // Build & Push Frontend 
                        sh "docker build -t ${DOCKER_USER}/kantin-frontend:latest ./frontend"[cite: 2, 6]
                        sh "docker push ${DOCKER_USER}/kantin-frontend:latest"
                    }
                }
            }
        }
        stage('Deploy to AKS') {
            steps {
                withCredentials([file(credentialsId: "${KUBE_CREDS}", variable: 'KUBE')]) {
                    sh "kubectl --kubeconfig=${KUBE} apply -f kantin-k8s.yaml.yml"[cite: 11]
                    sh "kubectl --kubeconfig=${KUBE} apply -f kantin-ingress.yaml"[cite: 10]
                }
            }
        }
    }
}
