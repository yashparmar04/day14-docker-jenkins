pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockercred')
        DOCKER_IMAGE = 'yashparmar04/day14'
        GIT_REPOSITORY = 'https://github.com/yashparmar04/day14-docker-jenkins.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${env.GIT_REPOSITORY}"
            }
        }

        stage('Setup Docker Buildx') {
            steps {
                sh '''
                    # Install Docker Buildx if not already installed
                    docker buildx version || docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
                    docker buildx create --use || docker buildx use default
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker buildx build --platform linux/amd64 -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', env.DOCKER_HUB_CREDENTIALS) {
                        sh 'docker buildx build --platform linux/amd64 --push -t ${DOCKER_IMAGE}:latest .'
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Example using Docker CLI to run the container
                    sh 'docker run -d -p 8085:8080 ${DOCKER_IMAGE}:latest'
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
