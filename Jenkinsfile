pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Cloner le dépôt Git
                git 'https://github.com/EllouzeEmna/ExamenDevops.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Récupérer la version basée sur le dernier commit
                    def version = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Construire l'image Docker pour le backend
                    sh "docker build -t mybackend:${version} ./spring-boot-server"
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
                    sh "docker build -t myfrontend:${version} ./angular-14-client"
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
                sh "scp docker-compose.yml recette@192.168.43.207:/path/to/project"
                // Démarrer les services Docker sur le serveur de test
                sh "ssh recette@192.168.43.207 'cd /path/to/project && docker-compose up -d'"
            }
        }
    }
}
