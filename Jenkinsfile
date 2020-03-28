pipeline {
    agent any

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            } //end steps
        }  // end Build

        stage('Run'){
            steps {
                sh "docker run -d -p 91${env.BUILD_ID}:8080 tomcatwebapp:${env.BUILD_ID}"
            } // end steps
        } // end Run
        
    } // end Stages
} // end pipeline