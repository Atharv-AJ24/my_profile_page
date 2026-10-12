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
                bat 'docker build -t my-profile-page:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker image...'
                bat 'docker image inspect my-profile-page:latest'
            }
        }

        
        
        stage('Check Kubernetes Config') {
            environment {
                KUBECONFIG = 'C:\\Users\\Admin\\.kube\\config'
            }
            steps {
                echo 'Checking Kubernetes configuration available to Jenkins...'
                bat 'where kubectl'
                bat 'kubectl config current-context'
                bat 'kubectl config view --minify -o jsonpath="{.clusters[0].cluster.server}"'
                bat 'kubectl get nodes --request-timeout=10s'
            }
        }



        stage('Deploy') {
            steps {
                echo 'Deploying application to Kubernetes...'
                bat 'kubectl apply -f kubernetes/deployment.yaml'
                bat 'kubectl apply -f kubernetes/service.yaml'
                bat 'kubectl rollout status deployment/my-profile-page --timeout=120s'
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