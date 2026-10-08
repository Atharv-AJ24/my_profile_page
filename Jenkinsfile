pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting project code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t my-profile-page:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker image...'
                sh 'docker images my-profile-page:latest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to Kubernetes...'
                sh 'kubectl apply -f kubernetes/deployment.yaml'
                sh 'kubectl apply -f kubernetes/service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}