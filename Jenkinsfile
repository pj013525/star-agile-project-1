pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/pj013525/star-agile-project-1/', branch: 'master'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t pj013525/finance-pro-image:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                    sh 'docker push pj013525/finance-pro-image:v1'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove existing container if running
                    sh 'docker rm -f finance-pro-cont || true'

                    // Run the container
                    sh 'docker run -d --name finance-pro-cont -p 8081:8080 pj013525/finance-pro-image:v1'
                }
            }
        }
    }
}
