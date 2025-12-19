pipeline {
    agent any

    environment {
        DOCKERHUB_CRED = credentials('dockerhub_id')
        IMAGE_REPO = "lankesh26/spring-welcome-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/lankehnagarajappa/spring-welcome-app.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_REPO:$IMAGE_TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_REPO:$IMAGE_TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop spring-app || true
                docker rm spring-app || true
                docker pull $IMAGE_REPO:$IMAGE_TAG
                docker run -d --name spring-app -p 9090:8080 $IMAGE_REPO:$IMAGE_TAG
                '''
            }
        }
    }
}

