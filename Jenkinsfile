pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/pj013525/star-agile-project-1/', branch: "master"
                sh 'mvn clean package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t pj013525/finance-pro-image:v1 .'
                    sh 'docker images'
                }
            }
        }
         
        
     stage('Deploy') {
            steps {
                sh 'sudo docker run -itd --name finance-pro-cont -p 8083:8081 pj013525/finance-pro-image:v1'
                  
                }
            }
        
    }
}
