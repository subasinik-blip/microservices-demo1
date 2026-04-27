pipeline {
    agent any

    environment {
        EC2_IP = '43.205.96.238'
        KEY_PATH = '/c/Users/DELL/Downloads/devops-key.pem'
    }

    stages {

        stage('Create Frontend File') {
            steps {
                writeFile file: 'index.html', text: '''
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>My Online Store</title>
<style>
body { font-family: Arial; margin:0; background:#f4f6f9; }
.header { background:#2c3e50; color:white; padding:15px; text-align:center; font-size:24px; }
.container { display:grid; grid-template-columns: repeat(3, 1fr); gap:20px; padding:20px; }
.card { background:white; padding:15px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.1); text-align:center; }
.card img { width:100%; border-radius:10px; }
.price { color:green; font-weight:bold; }
</style>
</head>
<body>

<div class="header">My Online Store</div>

<div class="container">
<div class="card">
<img src="https://picsum.photos/200?1">
<h3>Sunglasses</h3>
<p class="price">$25</p>
</div>

<div class="card">
<img src="https://picsum.photos/200?2">
<h3>Watch</h3>
<p class="price">$50</p>
</div>

<div class="card">
<img src="https://picsum.photos/200?3">
<h3>Headphones</h3>
<p class="price">$80</p>
</div>
</div>

</body>
</html>
'''
            }
        }

        stage('Copy File to EC2') {
            steps {
                bat '''
                scp -o StrictHostKeyChecking=no -i /c/Users/DELL/Downloads/devops-key.pem index.html ubuntu@43.205.96.238:/tmp/index.html
                '''
            }
        }

        stage('Deploy to Nginx') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i /c/Users/DELL/Downloads/devops-key.pem ubuntu@43.205.96.238 "sudo apt update || true && sudo apt install nginx -y && sudo systemctl start nginx && sudo mv /tmp/index.html /var/www/html/index.html"
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
