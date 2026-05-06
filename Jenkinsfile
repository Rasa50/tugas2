pipeline {
    agent any
    environment {
        DOCKER_USER = "rasarasanya"
        GIT_REPO_URL = "https://github.com/Rasa50/tugas2.git"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO_URL}"
            }
        }
        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-login', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "docker build -t ${USER}/tech-backend:latest ./backend"
                    sh "docker build -t ${USER}/tech-frontend:latest ./frontend"
                    sh "echo ${PASS} | docker login -u ${USER} --password-stdin"
                    sh "docker push ${USER}/tech-backend:latest"
                    sh "docker push ${USER}/tech-frontend:latest"
                }
            }
        }
        stage('Deploy ke Azure AKS') {
            steps {
                withKubeConfig([credentialsId: 'aks-config']) {
                    sh "kubectl apply -f k8s.yaml"
                    sh "kubectl apply -f ingress.yaml"
                    sh "kubectl rollout restart deployment backend-tech"
                    sh "kubectl rollout restart deployment frontend-tech"
                }
            }
        }
    }
}