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

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Publish & Deploy') {
            steps {
                sh 'mvn deploy'
            }
        }


        stage('Build Image') {
            steps {
                //sh 'docker build -t dockercorona/jenkinstest ./pushdockerimage/' (this will use the tag latest)
		sh 'docker build -t valvahen/finale:$BUILD_NUMBER ./pushdockerimage/'
            }
        }
        stage('Docker Login') {
            steps {
                //sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW' (this will leave the password visible)
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'                
                }
            }
        stage('Docker Push') {
            steps {
		//sh 'docker push dockercorona/jenkinstest' (this will use the tag latest)    
                sh 'docker push valvahen/finale:$BUILD_NUMBER'
                }
            }
        }
    

    post {
        always {
            sh 'docker logout'
        }
    }
}

