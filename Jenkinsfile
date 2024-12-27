pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'project2/yori:latest'
        CONTAINER_NAME = 'crazy_wilbur'
        PORT_MAPPING = '32768:80'  // Adjust the port mapping as needed
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

}