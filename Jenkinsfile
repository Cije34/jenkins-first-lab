pipeline {
    agent any
    
    stages {
        stage('Preparation') {
            steps {
                // Hapus container lama biar bersih
                sh 'docker rm -f my-web-server || true'
                // Kita tidak perlu "clean workspace" manual, Jenkins otomatis pull kode terbaru dari Git
            }
        }
        
        stage('Build Image') {
            steps {
                // Langsung build. Dockerfile & index.html sudah ada karena ditarik dari Git
                sh 'docker build -t my-simple-web:v2 .'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker run -d --name my-web-server -p 8082:80 my-simple-web:v2'
            }
        }
    }
}

