pipeline {
    agent any

    environment {
        IMAGE_NAME = 'monsite-Docker'
        CONTAINER_NAME = 'monsite-Docker-conteneur'
        HOST_PORT = '8081'
        CONTAINER_PORT = '80'
        GIT_REPO = 'https://github.com/Khaledik/jenkins-docker.git'
    }

    stages {
        stage('Cloner le repo') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Vérifier les fichiers') {
            steps {
                script {
                    if (!fileExists('Dockerfile') || !fileExists('index.html')) {
                        error("Dockerfile ou index.html manquant !")
                    }
                }
            }
        }

        stage('Build de l’image Docker') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Supprimer ancien conteneur') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Lancer le nouveau conteneur') {
            steps {
                sh """
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:${CONTAINER_PORT} \
                    ${IMAGE_NAME}
                """
            }
        }
    }
}
