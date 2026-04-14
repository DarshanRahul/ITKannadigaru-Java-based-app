pipeline {
    agent any

    tools {
        jdk 'java17'
        maven 'maven12'
    }

    environment {
        IMAGE_NAME = "darshanrahul/testdocker:${GIT_COMMIT}"
    }

    stages {

        // 🔹 Stage 1: Git Checkout
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DarshanRahul/ITKannadigaru-Java-based-app.git'
            }
        }

        // 🔹 Stage 2: Compile
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        // 🔹 Stage 3: Package
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        // 🔹 Stage 4: Docker Build
        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        // 🔹 Stage 5: Docker Run (Test)
      stage('Docker Test') {
    steps {
        sh '''
        docker rm -f testjenkinsss || true
        docker run -d --name testjenkinsss -p 8083:8080 ${IMAGE_NAME}
        '''
    }
}
		
		 stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Login to Docker Hub
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }  

        stage('Push to dockerhub'){
            steps{
                sh '''
                    docker push ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
    }
}
