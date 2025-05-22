pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/pj013525/star-agile-project-3.git'
            }
        }
        stage('Compilation') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Packing ') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build the image ') {
            steps {
                sh 'docker build -t pj013525/insurance-image:v1 .'
                sh 'docker images'
            }
        }
        stage('Push image to dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-details', variable: 'dockerhub_pwd')]) {
                sh 'echo "${dockerhub_pwd}" | docker login -u pj013525 -p ${dockerhub_pwd}'
                sh 'docker push pj013525/insurance-image:v1'
                }
            }
        }
        stage('Run the Container') {
            steps {
                sh 'docker run -id --name insurance-container -p 8082:8081 pj013525/insurance-image:v1'
            }
        }
    }
}
