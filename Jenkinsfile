pipeline {
    agent any
    environment {
        DOCKER_USER = "rasarasanya"
        GIT_REPO_URL = "https://github.com/Rasa50/tugas2.git"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: "${GIT_REPO_URL}"
            }
        }
        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-login', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    bat "docker build -t ${USER}/tech-backend:latest ./backend"
                    bat "docker build -t ${USER}/tech-frontend:latest ./frontend"
                    bat "echo %PASS% | docker login -u %USER% --password-stdin"
                    bat "docker push ${USER}/tech-backend:latest"
                    bat "docker push ${USER}/tech-frontend:latest"
                }
            }
        }
        stage('Deploy ke Azure AKS') {
            steps {
                withKubeConfig([credentialsId: 'aks-config']) {
                    bat "kubectl apply -f k8s.yaml"
                    bat "kubectl apply -f ingress.yaml"
                    bat "kubectl rollout restart deployment backend-tech"
                    bat "kubectl rollout restart deployment frontend-tech"
                }
            }
        }
    }
}