pipeline {
    agent any

    environment {
        BACKEND_IMAGE = 'ton-backend-image'   // <-- Remplace par le nom que tu veux pour ton image Docker
    }

    stages {
        stage('Build de l\'image Backend') {
            steps {
                bat "docker build -t %BACKEND_IMAGE%:latest ./Backend/odc"
            }
        }

        stage('Push de l\'image sur Docker Hub') {
            steps {
                bat """
                docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                docker tag %BACKEND_IMAGE%:latest %DOCKERHUB_USERNAME%/%BACKEND_IMAGE%:latest
                docker push %DOCKERHUB_USERNAME%/%BACKEND_IMAGE%:latest
                """
            }
        }

        stage('Déploiement local avec Docker Compose') {
            steps {
                bat "docker-compose up -d"
            }
        }
    }

    post {
        failure {
            echo 'Le pipeline a échoué.'
        }
        success {
            echo 'Le pipeline a réussi !'
        }
    }
}
