pipeline {
    agent any

    environment {

        DOCKERHUB_CREDENTIALS = credentials('DockerAuth')
    }

    stages {
        stage('Clone the project from GitHub') {
            steps {
                echo 'GitHub repository klonlanıyor...'
                git branch: 'main', url: 'https://github.com/mehmet1508/Devops-4.git'
            }
        }

        stage('Build the project') {
                    steps {
                        echo 'Proje derleniyor ve jar dosyasi oluşturuluyor...'
                        sh 'chmod +x gradlew'
                        sh './gradlew clean bootJar'
                    }
        }

        stage('Create a docker image') {
            steps {
                echo 'Docker imajı oluşturuluyor...'
                sh 'docker build -t mkerem1508/devops-4:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo 'DockerHub a giriş yapılıyor...'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push the image to the hub') {
            steps {
                echo 'Imaj DockerHub a yükleniyor...'
                sh 'docker push mkerem1508/devops-4:latest'
            }
        }

        stage('Run K8s deployment and service') {
            steps {
                echo 'Kubernetes deployment ve service çalıştırılıyor...'
                sh 'kubectl apply -f webapp-deploy.yaml'
                sh 'kubectl apply -f webapp-service.yaml'
                sh 'kubectl rollout restart deployment/devops-4'
            }
        }
    }
}