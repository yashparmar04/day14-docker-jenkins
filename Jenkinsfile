pipeline {
    agent any
    environment {
        imageName = 'yashparmar04/day14'
        tag = 'latest'
        dockerImage = ''
        containerName = 'day14-javaapp'
        dockerHubCredentials = 'dockercred'
    }

    stages {
        stage ('Checkout') {
            steps {
                git url: 'https://github.com/yashparmar04/day14-docker-jenkins.git', branch: 'main'
            }
        }
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build"${imageName}:${tag}"
                }
            }
        }
        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', dockerHubCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage ('Deploy Container') {
            steps {
                script {
                    sh "docker run -d -p 5051:80 --name ${containerName} ${imageName}:${tag}"
                }
            }
        }

    }
    post {
        success {
            echo 'Build and test succeeded!'
        }
        failure {
            echo 'Build or test failed!'
        }
    }
}
