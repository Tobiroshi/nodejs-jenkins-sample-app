pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-app"
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code depuis le dépôt
                checkout scm
            }
        }

        stage('Install deps') {
            steps {
                sh 'npm install'
            }
        }

        stage('Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Run container') {
            steps {
                sh '''
                    # On supprime l’ancien conteneur s’il existe
                    docker rm -f nodejs-container || true
                    # On lance le nouveau sur le port 3000
                    docker run -d --name nodejs-container -p 3000:3000 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }
}
