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
                    url: ''
            }
        }

        stage('Build des images') {
    steps {
        bat "docker build -t %BACKEND_IMAGE%:latest ./Backend/odc"
        bat "docker build -t %FRONTEND_IMAGE%:latest ./Frontend"
        bat "docker build -t %MIGRATE_IMAGE%:latest ./Backend/odc"
    }
}

        

       stage('Push des images sur Docker Hub') {
    steps {
        withDockerRegistry([credentialsId: 'docker_cred', url: '']) {
            bat "docker push %BACKEND_IMAGE%:latest"
            bat "docker push %FRONTEND_IMAGE%:latest"
            bat "docker push %MIGRATE_IMAGE%:latest"
        }
    }
}


        stage('Déploiement Local avec Docker Compose') {
            steps {
                bat '''
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            mail to: 'dieye6526@gmail.com',
                 subject: "✅ Déploiement local réussi!",
                 body: "L'application a été déployée localement avec succès."
        }
        failure {
            mail to: 'dieye6526@gmail.com',
                 subject: "❌ Échec du pipeline Jenkins",
                 body: "Une erreur s'est produite, merci de vérifier Jenkins."
        }
    }
