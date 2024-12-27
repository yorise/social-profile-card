pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rmn-profile:latest'
        CONTAINER_NAME = 'nervous_johnson'
        PORT_MAPPING = '8082:80'  // Adjust the port mapping as needed
    }

    stages {
        stage('Checkout') {
            steps {
                deleteDir() // Clean workspace
                checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/yorise/social-profile-card']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image with the HTML content
                    docker.build("${DOCKER_IMAGE}", '-f Dockerfile .')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker container based on the built image
                    docker.image("${DOCKER_IMAGE}").run("-p ${PORT_MAPPING} --name ${CONTAINER_NAME}")
                }
            }
        }
    }

    post {
        always {
            // Clean up the container after the build
            script {
                sh "docker rm -f ${CONTAINER_NAME} || true" // Ignore if the container doesn't exist
            }
        }
    }
}