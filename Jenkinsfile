pipeline {
    agent any

    environment {
        DOCKER_USER = 'cheikh9708'
        BACKEND_IMAGE = "${DOCKER_USER}/appprof-frontend"
        FRONTEND_IMAGE = "${DOCKER_USER}/appprof-backend"
        MIGRATE_IMAGE = "${DOCKER_USER}/appprof-migrate"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/diagnec/JenkinsProject.git'
            }
        }

        stage('Build des images') {
            steps {
                sh "docker build -t ${BACKEND_IMAGE}:latest ./Backend/odc"
                sh "docker build -t ${FRONTEND_IMAGE}:latest ./Frontend"
                sh "docker build -t ${MIGRATE_IMAGE}:latest ./Backend/odc"
            }
        }

        stage('Push des images sur Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'cheikhTocken', url: 'https://index.docker.io/v1/']) {
                    sh "docker push ${BACKEND_IMAGE}:latest"
                    sh "docker push ${FRONTEND_IMAGE}:latest"
                    sh "docker push ${MIGRATE_IMAGE}:latest"
                }
            }
        }

        stage('Déploiement Local avec Docker Compose') {
            steps {
                sh '''
                    docker-compose down || exit 0
                    docker-compose pull
                    docker-compose up -d --build
                '''
            }
        }
    }
}
