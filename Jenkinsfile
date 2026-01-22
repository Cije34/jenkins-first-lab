pipeline {
    agent any

   environment {
	IMAGE_NAME= 'cije34/my-simple-web'
	CONTAINER_NAME= 'my-web-server'
	DOCKER_CRED_ID= "docker-hub-reza"
	}	
    
    stages {
        stage('Preparation') {
            steps {
                // Hapus container lama biar bersih
                sh "docker rm -f ${CONTAINER_NAME} || true"
                // Kita tidak perlu "clean workspace" manual, Jenkins otomatis pull kode terbaru dari Git
            }
        }
        
        stage('Build Dynamic Image') {
            steps { 
                // Langsung build. Dockerfile & index.html sudah ada karena ditarik dari Git
		echo "building image version v1.0.${env.BUILD_NUMBER}..."
                sh "docker build -t ${IMAGE_NAME}:v1.0.${env.BUILD_NUMBER} ."
            }
        }

	stage("Push to Cloud"){
	    steps {
				echo "Uplouding to Docker"
		
				withCredentials([usernamePassword(credensialId: DOCKER_CRED_ID,usernameVariable: 'USER', passwordVariable: 'PASS')]){
		
				sh 'echo $PASS | docker login -u $USER --password-stdin'

				sh "docker push ${IMAGE_NAME}:v1.0.${env.BUILD_NUMBER}"

				sh 'docker logout'
			}	
		}
	
	}
        
        stage('Deploy (Local preview)') {
            steps {
                sh "docker run -d --name ${CONTAINER_NAME} -p 8082:80 ${IMAGE_NAME}:v1.0.${env.BUILD_NUMBER}"
            }
        }
    }
}

