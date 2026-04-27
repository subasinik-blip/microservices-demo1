pipeline {
    agent any

    environment {
        EC2_IP = '43.205.96.238'
        KEY_PATH = '/c/Users/DELL/Downloads/devops-key.pem'
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/subasinik-blip/microservices-demo1.git'
            }
        }

        stage('Terraform Init') {
            steps {
                dir('terraform') {
                    bat 'terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                dir('terraform') {
                    bat 'terraform apply -auto-approve'
                }
            }
        }

        stage('Connect EC2') {
            steps {
                bat """
                ssh -o StrictHostKeyChecking=no -i %KEY_PATH% ubuntu@%EC2_IP% "echo Connected to EC2"
                """
            }
        }

        stage('Run Command in EC2') {
            steps {
                bat """
                ssh -o StrictHostKeyChecking=no -i %KEY_PATH% ubuntu@%EC2_IP% "sudo apt update && echo Setup Done"
                """
            }
        }
    }
}
