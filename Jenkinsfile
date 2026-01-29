pipeline {
    agent any

    environment {
        IMAGE_NAME = "hello-ci:${BUILD_NUMBER}"
        CONTAINER_NAME = "hello-ci-run"
        DOCKERHUB_CREDS = credentials('dockerhub-creds')
    }

    stages {
        stage('Checkout (Git)') {
            steps {
                checkout scm
            }
        }

        stage('Build (Maven)') {
            steps {
                sh 'mvn -v'
                sh 'mvn clean package'
            }
        }

        stage('Build Image (Docker)') {
            steps {
                sh 'docker version'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run (Docker)') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        always {
            sh 'docker rm -f $CONTAINER_NAME || true'
            sh 'docker image prune -f || true'
        }
    }
}
