pipeline {
    agent {
        docker {
            image 'docker:19.03.12'
            args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        DOCKER_CREDENTIALS_ID = 'dockercred'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/yashparmar04/day14-docker-jenkins.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build('yashparmar04/day14')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "$DOCKER_CREDENTIALS_ID") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    dockerImage.run('-d -p 8085:80')
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
