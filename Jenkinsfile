pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/service-a.git'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("service-a:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    dockerImage.push()
                    sh 'kubectl apply -f k8s/service-a-deployment.yml -n staging'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                input "Deploy to production?"
                script {
                    sh 'kubectl apply -f k8s/service-a-deployment.yml -n production'
                }
            }
        }
    }
}
