pipeline {
    agent any
    environment {
        DOCKERHUB = credentials('dockerhub-creds')
        IMAGE = 'revets/order-ms:latest'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }
        stage('Docker Login and Push') {
            steps {
                sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'
                sh 'docker push $IMAGE'
            }
        }
        stage('Start Deploy Pipeline') {
            steps {
                build job: 'deploy_order_ms_eks', parameters: [
                    string(name: 'IMAGE', value: "${IMAGE}")
                ]
            }
        }
    }
}