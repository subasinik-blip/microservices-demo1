pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "subasinik/frontend:${BUILD_NUMBER}"
        DOCKER_LATEST = "subasinik/frontend:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/subasinik-blip/microservices-demo1.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                cd src/frontend
                docker build -t $DOCKER_IMAGE -t $DOCKER_LATEST .
                '''
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-secret') {
                        sh '''
                        docker push $DOCKER_IMAGE
                        docker push $DOCKER_LATEST
                        '''
                    }
                }
            }
        }
    }
}
