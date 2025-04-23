pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'
    }

    environment {
        DOCKER_IMAGE = "balajee2k/spring-petclinic:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Balajee2k/spring-petclinic.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials-id']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/petclinic workload=$DOCKER_IMAGE'
            }
        }
    }
}
