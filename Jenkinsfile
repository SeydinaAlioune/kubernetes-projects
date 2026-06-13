pipeline {
    agent any

    stages {
        stage('1. Récupération du Code') {
            steps {
                checkout scm
                echo '✅ Code récupéré avec succès.'
            }
        }
        
        stage('2. Scan de Sécurité (Trivy sur Kubernetes)') {
            steps {
                echo '🛡️ Installation et Scan avec Trivy...'
                // On télécharge Trivy directement dans Jenkins pour éviter le problème de volume Docker
                sh '''
                curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b .
                ./trivy config --exit-code 0 ./03-java-app-deployment
                '''
                echo '✅ Scan terminé.'
            }
        }
    }
    
    post {
        always {
            emailext (
                subject: "Rapport de Sécurité Jenkins : ${currentBuild.currentResult}",
                body: "Le scan Trivy est terminé. \nStatut du pipeline : ${currentBuild.currentResult}. \n\nConsulte les logs ici : ${env.BUILD_URL}console",
                to: 'diaoseydina62@gmail.com'
            )
        }
    }
}
