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
                dir('C:/Users/DELL/terraform-aws-project') {
                    bat 'terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                dir('C:/Users/DELL/terraform-aws-project') {
                    bat 'terraform apply -auto-approve'
                }
            }
        }

        stage('Connect EC2') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i /c/Users/DELL/Downloads/devops-key.pem ubuntu@43.205.96.238 "echo Connected"
                '''
            }
        }

        stage('Deploy Frontend UI') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i /c/Users/DELL/Downloads/devops-key.pem ubuntu@43.205.96.238 "sudo apt update || true && sudo apt install nginx -y && sudo systemctl start nginx && echo '<!DOCTYPE html><html><head><title>Online Store</title><style>body { font-family: Arial; background:#f4f6f9; margin:0; } .header { background:#2c3e50; color:white; padding:15px; text-align:center; font-size:24px; } .container { display:grid; grid-template-columns: repeat(3, 1fr); gap:20px; padding:20px; } .card { background:white; padding:15px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.1); text-align:center; } .card img { width:100%; border-radius:10px; } .price { color:green; font-weight:bold; }</style></head><body><div class=\"header\">🛒 My Online Store</div><div class=\"container\"><div class=\"card\"><img src=\"https://via.placeholder.com/200\"><h3>Sunglasses</h3><p class=\"price\">$25</p></div><div class=\"card\"><img src=\"https://via.placeholder.com/200\"><h3>Watch</h3><p class=\"price\">$50</p></div><div class=\"card\"><img src=\"https://via.placeholder.com/200\"><h3>Headphones</h3><p class=\"price\">$80</p></div></div></body></html>' | sudo tee /var/www/html/index.html"
                '''
            }
        }

        stage('Verify Server') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i /c/Users/DELL/Downloads/devops-key.pem ubuntu@43.205.96.238 "systemctl is-active nginx"
                '''
            }
        }
    }
}
