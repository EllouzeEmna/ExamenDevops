pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'emnaellouze123487'  // Docker Hub username
        DOCKERHUB_PASSWORD = credentials('DockerHubPassword')  // Docker Hub password credentials
        VM2_USER = 'recette'           // SSH user for VM2
        VM2_IP = '192.168.43.207'     // IP address of VM2
        VM2_APP_PATH = '~/app'        // Path to deploy on VM2
        DOCKER_CLI_EXPERIMENTAL = "enabled"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/EllouzeEmna/ExamenDevops.git', branch: 'main'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    def backendVersion = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    sh "docker build -t ${DOCKERHUB_USERNAME}/mybackend:${backendVersion} ./spring-boot-server"
                }
            }
        }

        stage('Push Backend Docker Image') {
            steps {
                script {
                    def backendVersion = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    sh "docker push ${DOCKERHUB_USERNAME}/mybackend:${backendVersion}"
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    def frontendVersion = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    sh "docker build -t ${DOCKERHUB_USERNAME}/myfrontend:${frontendVersion} ./angular-14-client"
                }
            }
        }

        stage('Push Frontend Docker Image') {
            steps {
                script {
                    def frontendVersion = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    sh "docker push ${DOCKERHUB_USERNAME}/myfrontend:${frontendVersion}"
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                sh '''
                docker-compose up -d
                docker-compose exec -T spring-boot-server  
                docker-compose down
                '''
            }
        }

        stage('Run Frontend Tests') {
            steps {
                sh '''
                docker-compose up -d
                docker-compose exec angular-14-client 
                docker-compose down
                '''
            }
        }

        stage('Deploy on VM2') {
            steps {
                sshagent(['ssh-credentials-ci']) {
                    sh '''
                    # Create the app directory on the remote machine
                    ssh -o StrictHostKeyChecking=no $VM2_USER@$VM2_IP "mkdir -p $VM2_APP_PATH"
        
                    # Copy the docker-compose.yml file
                    scp docker-compose.yml $VM2_USER@$VM2_IP:$VM2_APP_PATH/
        
                    # Execute Docker Compose on the remote machine
                    ssh -o StrictHostKeyChecking=no $VM2_USER@$VM2_IP "
                        cd $VM2_APP_PATH &&
                        sudo docker-compose pull &&
                        sudo docker-compose up -d --build
                    "
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
