
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
	   steps {
        	sh '''
            	docker run --rm \
                	--volumes-from jenkins \
                	-w "$WORKSPACE" \
               		 node:20-alpine npm install
        	'''
    		}
	}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t health-plus:v${BUILD_NUMBER} .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f health-plus-container || true

                    docker run -d \
                        --name health-plus-container \
                        -p 3000:3000 \
                        health-plus:v${BUILD_NUMBER}
                '''
            }
        }
    }
}

