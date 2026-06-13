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
                echo '🛡️ Scan des fichiers .yml avec Trivy...'
                // Trivy analyse directement le dossier contenant tes fichiers Kubernetes
                sh 'docker run --rm -v ${WORKSPACE}:/workspace -w /workspace aquasec/trivy config --exit-code 1 ./03-java-app-deployment'
                echo '✅ Scan terminé.'
            }
        }
    }
    
    post {
        always {
            // L'envoi automatique de l'e-mail à la fin du build !
            emailext (
                subject: "Rapport de Sécurité Jenkins : ${currentBuild.currentResult}",
                body: "Le scan Trivy est terminé. \nStatut du pipeline : ${currentBuild.currentResult}. \n\nPour voir quelles failles ont été trouvées, consulte les logs ici : ${env.BUILD_URL}console",
                to: 'diaoseydina62@gmail.com' // ⚠️ N'oublie pas de mettre ton adresse mail ici
            )
        }
    }
}
