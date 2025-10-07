pipeline {
    agent any  // Corre en cualquier agente (pod en K3s)
    
    stages {    
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Angel-Guxman/niux-parcelas.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'  // Build tu app
                sh 'docker build -t express-parcelas:latest .'
                sh 'docker push elyxium/express-parcelas:latest'  // A tu registry
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy to K3s') {
            steps {
                withKubeConfig([credentialsId: 'mi-k3s-kubeconfig']) {  // Secret de Jenkins
                    sh 'kubectl apply -f k8s/deployment.yaml'  // Aplica manifests
                    sh 'kubectl rollout status deployment/tu-app'
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminado!'
        }
        success {
            echo 'Deploy OK - chequea Grafana!'
        }
        failure {
            echo 'Falló - revisa logs.'
        }
    }
}