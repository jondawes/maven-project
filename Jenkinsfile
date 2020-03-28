pipeline {
    agent any

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        parallel {
            stage('Build'){
                steps {
                    sh 'mvn clean package'
                    sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                } //end steps
            }  // end Build

            stage ( 'Static Analysis'){
                steps {
                    sh 'mvn checkstyle:checkstyle'
                }
                post {
                    always {
                        recordIssues enabledForFailure: true, tool: checkStyle()
                    }
                }
            }  //end Stage

        } // end parallel

        stage('Run'){
            steps {
                sh "docker run -d -p 91${env.BUILD_ID}:8080 tomcatwebapp:${env.BUILD_ID}"
            } // end steps
        } // end Run
        
    } // end Stages
} // end pipeline