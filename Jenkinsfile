pipeline {
    agent any

    environment {
        EC2_IP = '43.205.96.238'
        KEY_PATH = '/c/Users/DELL/Downloads/devops-key.pem'
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/subasinik-blip/microservices-demo1.git'
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

        stage('Setup EC2 Server') {
            steps {
                bat """
                ssh -o StrictHostKeyChecking=no -i %KEY_PATH% ubuntu@%EC2_IP% "
                sudo apt update &&
                sudo apt install nginx -y &&
                sudo systemctl start nginx &&
                echo Server Setup Complete
                "
                """
            }
        }

        stage('Verify Server') {
            steps {
                bat """
                ssh -o StrictHostKeyChecking=no -i %KEY_PATH% ubuntu@%EC2_IP% "
                systemctl status nginx | grep active
                "
                """
            }
        }
    }
}
