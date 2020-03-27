pipeline {
    agent any
    stages {
        stage('Build'){
            steps{
                sh 'mvn clean package'
                sh "sudo docker build . -t tomvatwebapp:${env.BUILD_ID}"
            }
        }
    }
}