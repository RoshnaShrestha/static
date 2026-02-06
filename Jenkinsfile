pipeline {
    agent any

    triggers {
        // Polls the SCM (GitHub/GitLab) every 5 minutes
        pollSCM('H/5 * * * *')
    }

    environment {
        DOCKER_IMAGE = "my-modern-site"
    }

    stages {
        stage('Checkout') {
            steps {
                // Jenkins automatically checks out code, but we can verify it here
                echo 'Checking out source code...'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Builds the image using the Dockerfile in the current directory
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stops any existing container on port 8080 and starts the new one
                    sh "docker stop web-container || true"
                    sh "docker rm web-container || true"
                    sh "docker run -d --name web-container -p 8080:80 ${DOCKER_IMAGE}:${env.BUILD_ID}"
                }
            }
        }
    }

    post {
        success {
            echo "Successfully deployed! Access at http://localhost:1070"
        }
    }
}