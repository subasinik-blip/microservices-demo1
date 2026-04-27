pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/subasinik-blip/microservices-demo1.git'
            }
        }

        stage('Terraform') {
            steps {
                dir('terraform') {
                    bat 'terraform init'
                    bat 'terraform apply -auto-approve'
                }
            }
        }
    }
}
