pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhub/frontend:${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/subasinik-blip/microservices-demo1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'cd src/frontend && go build -o app .'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'cd src/frontend && docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-secret') {
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }
    }
}
