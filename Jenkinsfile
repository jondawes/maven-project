pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }  // end Build

        stage('Run'){
            sh "docker run -d -p 91${env.BUILD_ID}:8080 tomcatwebapp:${env.BUILD_ID}"

        } // end Run
        
    }
}