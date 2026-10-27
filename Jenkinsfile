
pipeline {
    agent any

    stages {
        stage('Validate Kubernetes') {
            steps {
                bat 'kubectl version --client'
            }
        }

        stage('Deploy Web Application') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl apply -f deployment.yaml'
                    bat 'kubectl rollout status deployment/webapp-deployment --timeout=120s'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl get deployments'
                    bat 'kubectl get pods'
                    bat 'kubectl get services'
                }
            }
        }
    }
}
