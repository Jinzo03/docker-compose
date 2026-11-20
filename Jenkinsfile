pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        // Create this Jenkins credential as a username/password credential.
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKERHUB_USERNAME = "${DOCKERHUB_CREDENTIALS_USR}"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Build Docker images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Push images to Docker Hub') {
            steps {
                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login \
                        --username "$DOCKERHUB_USERNAME" --password-stdin
                    docker compose push frontend backend
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                // Detached mode lets Jenkins continue after starting the stack.
                sh 'docker compose up -d --no-build'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
