pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE    = "jenkins-demo-app"
        DOCKER_TAG      = "${BUILD_NUMBER}"
        CONTAINER_NAME  = "jenkins-demo-container"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // TODO: Récupérer le code source
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // TODO: Installer les dépendances
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                // TODO: Lancer les tests
                sh 'npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // TODO: Analyser le code avec SonarQube
                withSonarQubeEnv('sonar-server') {
                    sh 'sonar-scanner'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // TODO: Construire l\'image Docker
                sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
            }
        }
        
        stage('Deploy') {
            steps {
                // TODO: Déployer le conteneur
                // Arrêter l'ancien conteneur s'il existe 
                // Démarrer le nouveau conteneur avec la nouvelle version
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}
                """
                }
            }
        }
    
        post {
            success {
                // TODO: Nettoyer les anciennes images Docker
                // Ici on supprime les images "dangling" non utilisées
                sh 'docker image prune -f'
                }    
            }
    }
