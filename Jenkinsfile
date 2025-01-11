pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Cloner le dépôt Git
                git 'https://gitlab.com/ahmed.jemal.sfax.iset/etude_de_cas2.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Récupérer la version basée sur le dernier commit
                    def version = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Construire l'image Docker pour le backend
                    sh "docker build -t mybackend:${version} ./backend"
                }
            }
        }

        stage('Push Backend Docker Image') {
            steps {
                script {
                    // Récupérer la version basée sur le dernier commit
                    def version = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Pousser l'image Docker du backend vers le registre
                    sh "docker push mybackend:${version}"
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Récupérer la version basée sur le dernier commit
                    def version = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Construire l'image Docker pour le frontend
                    sh "docker build -t myfrontend:${version} ./frontend"
                }
            }
        }

        stage('Push Frontend Docker Image') {
            steps {
                script {
                    // Récupérer la version basée sur le dernier commit
                    def version = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Pousser l'image Docker du frontend vers le registre
                    sh "docker push myfrontend:${version}"
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                // Copier le fichier docker-compose.yml vers le serveur de test
                sh "scp docker-compose.yml user@recette-server:/path/to/project"
                // Démarrer les services Docker sur le serveur de test
                sh "ssh user@recette-server 'cd /path/to/project && docker-compose up -d'"
            }
        }
    }
}
