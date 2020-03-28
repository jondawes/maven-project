pipeline {
    agent any

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Testing') {
            parallel {
                stage('Build'){
                    steps {
                        sh 'mvn clean package'
                        sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                    } //end steps
                }  // end stage Build

                stage ( 'Static Analysis'){
                    steps {
                        sh 'mvn checkstyle:checkstyle'
                    }
                    post {
                        always {
                            recordIssues enabledForFailure: true, tool: checkStyle()
                        }
                    }
                }  //end Stage static analysis

            } // end parallel
        } // end stage testing

        stage('Run'){
            steps {
                sh "docker run -d -p 91${env.BUILD_ID}:8080 tomcatwebapp:${env.BUILD_ID}"
            } // end steps
        } // end Run
        
    } // end Stages
} // end pipeline