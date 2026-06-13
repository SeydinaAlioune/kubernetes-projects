pipeline {
    agent any

    stages {
        stage('1. Récupération du Code') {
            steps {
                checkout scm
                echo '✅ Code récupéré avec succès.'
            }
        }
        
        stage('2. Construction (Image Docker)') {
            steps {
                // Jenkins entre dans le bon dossier avant de lancer la commande Docker
                dir('03-java-app-deployment') {
                    sh 'docker build -t k8s-app-sec:latest .'
                }
                echo '✅ Image Docker construite.'
            }
        }
        
        stage('3. Scan de Sécurité (Trivy)') {
            steps {
                echo '🛡️ Lancement du scan avec Trivy...'
                // Jenkins scanne l'image qu'il vient de construire
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --exit-code 1 --severity HIGH,CRITICAL k8s-app-sec:latest'
                echo '✅ Scan terminé avec succès.'
            }
        }
    }
    
    post {
        success {
            echo '🎉 Pipeline réussi et image sécurisée !'
        }
        failure {
            echo '❌ Le build a échoué (Soit erreur Docker, soit Trivy a trouvé des failles !)'
        }
    }
}
